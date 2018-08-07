---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}




# Ouverture des ports et adresses IP requis dans votre pare-feu
{: #firewall}

Examinez ces situations pour lesquelles vous aurez peut-être à ouvrir des ports et des adresses IP spécifiques dans vos pare-feux pour {{site.data.keyword.containerlong}} :
{:shortdesc}

* [Pour exécuter des commandes `bx`](#firewall_bx) depuis votre système local lorsque les règles réseau de l'entreprise empêchent l'accès à des noeuds finaux Internet publics via des proxies ou des pare-feux.
* [Pour exécuter des commandes `kubectl`](#firewall_kubectl) depuis votre système local lorsque les règles réseau de l'entreprise empêchent l'accès à des noeuds finaux Internet publics via des proxies ou des pare-feux.
* [Pour exécuter des commandes `calicoctl`](#firewall_calicoctl) depuis votre système local lorsque les règles réseau de l'entreprise empêchent l'accès à des noeuds finaux Internet publics via des proxies ou des pare-feux.
* [Pour autoriser la communication entre le maître Kubernetes et les noeuds worker ](#firewall_outbound)lorsqu'un pare-feu a été mis en place pour les noeuds worker ou que les paramètres du pare-feu ont été personnalisés dans votre compte d'infrastructure IBM Cloud (SoftLayer).
* [Pour accéder au service NodePort, au service LoasBalancer, ou à Ingress de l'extérieur du cluster](#firewall_inbound).

<br />


## Exécution  de commandes `bx cs` derrière un pare-feu
{: #firewall_bx}

Si les règles réseau de l'entreprise empêchent l'accès depuis votre système à des noeuds finaux publics via des proxies ou des pare-feux, pour exécuter des commandes `bx cs`, vous devez autoriser l'accès TCP pour {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

1. Autorisez l'accès à `containers.bluemix.net` sur le port 443.
2. Vérifiez votre connexion. Si l'accès est configuré correctement, des navires sont affichés dans la sortie.
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   Exemple de sortie :
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## Exécution de commandes `kubectl` derrière un pare-feu
{: #firewall_kubectl}

Si les règles réseau de l'entreprise empêchent l'accès depuis votre système à des noeuds finaux publics via des proxies ou des pare-feux, pour exécuter des commandes `kubectl`, vous devez autoriser l'accès TCP pour le cluster.
{:shortdesc}

Lorsqu'un cluster est créé, le port dans l'URL maîtresse est affecté aléatoirement sur la plage 20000 à 32767. Vous pouvez choisir d'ouvrir la plage de ports 20000 à 32767 pour n'importe quel cluster pouvant être créé ou bien autoriser l'accès pour un cluster existant spécifique.

Avant de commencer, autorisez l'accès aux commandes [run `bx cs`](#firewall_bx).

Pour autoriser l'accès à un cluster spécifique :

1. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}}. Si vous disposez d'un compte fédéré, incluez l'option `--sso`.

   ```
   bx login [--sso]
   ```
   {: pre}

2. Sélectionnez la région où réside votre cluster.

   ```
   bx cs region-set
   ```
   {: pre}

3. Obtenez le nom de votre cluster.

   ```
   bx cs clusters
   ```
   {: pre}

4. Extrayez la valeur **Master URL** pour votre cluster.

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   Exemple de sortie :
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. Autorisez l'accès à l'URL maître (**Master URL**) sur le port (comme au port `31142` dans l'exemple précédent).

6. Vérifiez votre connexion.

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   Exemple de commande :
   ```
   curl --insecure https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   Exemple de sortie :
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. Facultatif : répétez ces étapes pour chaque cluster que vous avec besoin d'exposer.

<br />


## Exécution de commandes `calicoctl` derrière un pare-feu
{: #firewall_calicoctl}

Si les règles réseau de l'entreprise empêchent l'accès depuis votre système à des noeuds finaux publics via des proxies ou des pare-feux, pour exécuter des commandes `calicoctl`, vous devez autoriser l'accès TCP pour les commandes Calico.
{:shortdesc}

Avant de commencer, autorisez l'accès pour exécution de commandes [`bx`](#firewall_bx) et [`kubectl`](#firewall_kubectl).

1. Extrayez l'adresse IP de l'URL maîtresse que vous avez utilisée pour autoriser les commandes [`kubectl`](#firewall_kubectl).

2. Obtenez le port pour ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Autorisez l'accès pour les règles Calico via l'adresse IP et le port ETCD de l'URL maîtresse.

<br />


## Autorisation au cluster d'accéder aux ressources de l'infrastructure et à d'autres services
{: #firewall_outbound}

Laissez votre cluster accéder aux ressources d'infrastructure et aux services derrière un pare-feu, comme pour les régions {{site.data.keyword.containershort_notm}}, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, les adresses IP privées de l'infrastructure IBM Cloud (SoftLayer) et l'accès sortant pour les réservations de volume persistant.
{:shortdesc}

  1.  Notez l'adresse IP publique pour tous vos noeuds worker dans le cluster.

      ```
      bx cs workers <cluster_name_or_ID>
      ```
      {: pre}

  2.  Autorisez le trafic entrant depuis la source _<each_worker_node_publicIP>_ vers la plage de ports TCP/UDP de destination 20000 à 32767 et le port 443, et aux adresses IP et groupes réseau suivants. Si un pare-feu d'entreprise empêche votre machine locale d'accéder à des noeuds finaux Internet publics, effectuez cette étape tant pour vos noeuds worker source que pour votre machine locale.
      - **Important** : vous devez autoriser le trafic sortant vers le port 443 pour tous les emplacements de la région afin d'équilibrer la charge lors du processus d'amorçage. Par exemple, si votre cluster se trouve au Sud des Etats-Unis, vous devez autoriser le trafic du port 443 vers les adresses IP de les emplacements (dal10, dal12 et dal13).
      <p>
  <table summary="La première ligne du tableau s'étend sur deux colonnes. Les autres lignes se lisent de gauche à droite. L'emplacement du serveur figure dans la première colonne et les adresses IP pour concordance dans la seconde colonne.">
  <caption>Adresses IP à ouvrir pour le trafic sortant</caption>
      <thead>
      <th>Région</th>
      <th>Emplacement</th>
      <th>Adresse IP</th>
      </thead>
    <tbody>
      <tr>
        <td>Asie-Pacifique nord</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>Asie-Pacifique sud</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>Europe centrale</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>Sud du Royaume-Uni</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>Est des Etats-Unis</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>Sud des Etats-Unis</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  Autorisez le trafic réseau sortant depuis les noeuds worker vers les [régions {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_overview.html#registry_regions):
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Remplacez <em>&lt;registry_publicIP&gt;</em> par les adresses IP du registre auxquelles vous désirez autoriser le trafic. Le registre global héberge des images publiques fournies par IBM et les registres régionaux vos propres images privées ou publiques.
        <p>
<table summary="La première ligne du tableau s'étend sur deux colonnes. Les autres lignes se lisent de gauche à droite. L'emplacement du serveur figure dans la première colonne et les adresses IP pour concordance dans la seconde colonne.">
  <caption>Adresses IP à ouvrir pour le trafic du registre</caption>
      <thead>
        <th>Région {{site.data.keyword.containershort_notm}}</th>
        <th>Adresse du registre</th>
        <th>Adresse IP du registre</th>
      </thead>
      <tbody>
        <tr>
          <td>Registre global entre les régions d'{{site.data.keyword.containershort_notm}}</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>Asie-Pacifique nord, Asie-Pacifique sud</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>Europe centrale</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>Sud du Royaume-Uni</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>Est des Etats-Unis, Sud des Etats-Unis</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  Facultatif : autorisez le trafic réseau sortant depuis les noeuds worker vers les services {{site.data.keyword.monitoringlong_notm}} et {{site.data.keyword.loganalysislong_notm}} :
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - Remplacez <em>&lt;monitoring_public_IP&gt;</em> par toutes les adresses des régions de surveillance auxquelles vous voulez autoriser le trafic :
        <p><table summary="La première ligne du tableau s'étend sur deux colonnes. Les autres lignes se lisent de gauche à droite. L'emplacement du serveur figure dans la première colonne et les adresses IP pour concordance dans la seconde colonne.">
  <caption>Adresses IP à ouvrir pour gérer le trafic</caption>
        <thead>
        <th>Région {{site.data.keyword.containershort_notm}}</th>
        <th>Adresse de surveillance</th>
        <th>Adresses IP de surveillance</th>
        </thead>
      <tbody>
        <tr>
         <td>Europe centrale</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>Sud du Royaume-Uni</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>Est des Etats-Unis, Sud des Etats-Unis, Asie-Pacifique nord</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - Remplacez <em>&lt;logging_public_IP&gt;</em> par toutes les adresses des régions de consignation auxquelles vous voulez autoriser le trafic :
        <p><table summary="La première ligne du tableau s'étend sur deux colonnes. Les autres lignes se lisent de gauche à droite. L'emplacement du serveur figure dans la première colonne et les adresses IP pour concordance dans la seconde colonne.">
<caption>Adresses IP à ouvrir pour consigner le trafic</caption>
        <thead>
        <th>Région {{site.data.keyword.containershort_notm}}</th>
        <th>Adresse de consignation</th>
        <th>Adresses IP de consignation</th>
        </thead>
        <tbody>
          <tr>
            <td>Est des Etats-Unis, Sud des Etats-Unis</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>Sud du Royaume-Uni</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>Europe centrale</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>Asie-Pacifique sud, Asie-Pacifique nord</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. Dans le cas de pare-feux privés, autorisez les plages d'adresses IP privées d'infrastructure IBM Cloud (SoftLayer) appropriées. Consultez [ce lien](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) en commençant par la section **Backend (private) Network**.
      - Ajoutez tous les [emplacements dans les régions](cs_regions.html#locations) que vous utilisez.
      - Notez que vous devez ajouter l'emplacement dal01 (centre de données).
      - Ouvrez les ports 80 et 443 pour permettre le processus d'amorçage de cluster.

  6. {: #pvc}Pour créer des réservations de volume persistant pour le stockage de données, permettez un accès sortant via votre pare-feu aux [adresses IP de l'infrastructure IBM Cloud (SoftLayer)](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) de l'emplacement (centre de données) où réside votre cluster.
      - Pour déterminer l'emplacement (centre de données) de votre cluster, exécutez la commande `bx cs clusters`.
      - Autorisez l'accès à la plage d'adresses IP tant pour le réseau **Frontend (public) network** que **Backend (privé)**.
      - Notez que vous devez ajouter l'emplacement dal01 (centre de données) pour le réseau **Backend (privé)**.

<br />


## Accès à NodePort, à l'équilibreur de charge et aux services Ingress de l'extérieur du cluster
{: #firewall_inbound}

Vous pouvez autoriser l'accès entrant au NodePort, à l'équilibreur de charge et aux services Ingress.
{:shortdesc}

<dl>
  <dt>Service NodePort</dt>
  <dd>Ouvrez le port que vous avez configuré lorsque vous avez déployé le service sur les adresses IP publiques pour tous les noeuds worker vers lesquels autoriser le trafic. Pour identifier le port, exécutez la commande `kubectl get svc`. Le port est situé sur la plage 20000 à 32000.<dd>
  <dt>Service LoadBalancer</dt>
  <dd>Ouvrez le port que vous avez configuré lorsque vous avez déployé le service sur l'adresse IP publique du service d'équilibrage de charge.</dd>
  <dt>Ingress</dt>
  <dd>Ouvrez le port 80 pour HTTP ou le port 443 pour HTTPS vers l'adresse IP de l'équilibreur de charge d'application Ingress.</dd>
</dl>
