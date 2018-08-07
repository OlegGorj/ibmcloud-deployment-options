---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Logging and monitoring Ingress
{: #ingress_health}

Customize logging and set up monitoring to help you troubleshoot issues and improve the performance of your Ingress configuration.
{: shortdesc}

## Customizing Ingress log content and format
{: #ingress_log_format}

You can customize the content and format of logs that are collected for the Ingress ALB.
{:shortdesc}

By default, Ingress logs are formatted in JSON and display common log fields. However, you can also create a custom log format. To choose which log components are forwarded and how the components are arranged in the log output:

1. Create and open a local version of the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Add a <code>data</code> section. Add the `log-format` field and optionally, the `log-format-escape-json` field.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the log-format configuration</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Replace <code>&lt;key&gt;</code> with the name for the log component and <code>&lt;log_variable&gt;</code> with a variable for the log component that you want to collect in log entries. You can include text and punctuation that you want the log entry to contain, such as quotation marks around string values and commas to separate log components. For example, formatting a component like <code>request: "$request"</code> generates the following in a log entry: <code>request: "GET / HTTP/1.1"</code> . For a list of all the variables you can use, see the <a href="http://nginx.org/en/docs/varindex.html">Nginx variable index</a>.<br><br>To log an additional header such as <em>x-custom-ID</em>, add the following key-value pair to the custom log content: <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>Hyphens (<code>-</code>) are converted to underscores (<code>_</code>) and <code>$http_</code> must be prepended to the custom header name.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Optional: By default, logs are generated in text format. To generate logs in JSON format, add the <code>log-format-escape-json</code> field and use value <code>true</code>.</td>
    </tr>
    </tbody></table>

    For example, your log format might contain the following variables:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    A log entry according to this format looks like the following example:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    To create a custom log format that is based on the default format for ALB logs, modify the following section as needed and add it to your configmap:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Save the configuration file.

5. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. To view the Ingress ALB logs, choose between two options.
    * [Create a logging configuration for the Ingress service](cs_health.html#logging) in your cluster.
    * Check the logs from the CLI.
        1. Get the ID of a pod for an ALB.
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. Open the logs for that ALB pod. Verify that logs follow the updated format.
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />




## Increasing the shared memory zone size for Ingress metrics collection
{: #vts_zone_size}

Shared memory zones are defined so that worker processes can share information such as cache, session persistence, and rate limits. A shared memory zone, called the virtual host traffic status zone, is set up for Ingress to collect metrics data for an ALB.
{:shortdesc}

In the `ibm-cloud-provider-ingress-cm` Ingress configmap, the `vts-status-zone-size` field sets the size of the shared memory zone for metrics data collection. By default, `vts-status-zone-size` is set to `10m`. If you have a large environment that requires more memory for metrics collection, you can override the default to instead use a larger value by following these steps.

1. Create and open a local version of the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Change the value of `vts-status-zone-size` from `10m` to a larger value.

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Save the configuration file.

4. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
