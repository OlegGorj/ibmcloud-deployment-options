---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer)
{: #infrastructure}

Per creare un cluster Kubernetes standard, devi avere accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Questo accesso è necessario per richiedere le risorse di infrastruttura a pagamento, come nodi di lavoro, indirizzi IP pubblici portatili o archiviazione persistente per il tuo cluster Kubernetes in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer)
{: #unify_accounts}

Gli account Pagamento a consumo di {{site.data.keyword.Bluemix_notm}} che sono stati creati dopo l'abilitazione del collegamento automatico degli account sono già configurati con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Puoi acquistare le risorse dell'infrastruttura per il tuo cluster senza ulteriori configurazioni.
{:shortdesc}

Gli utenti con altri tipi di account {{site.data.keyword.Bluemix_notm}} o quelli che hanno un account dell'infrastruttura IBM Cloud (SoftLayer) esistente non collegato al loro account {{site.data.keyword.Bluemix_notm}}, devono configurare i propri account per creare i cluster standard.
{:shortdesc}

Esamina la seguente tabella per trovare le opzioni disponibili per ogni tipo di account.

|Tipo di account|Descrizione|Opzioni disponibili per creare un cluster standard|
|------------|-----------|----------------------------------------------|
|Account Lite|Gli account lite non possono eseguire il provisioning dei cluster.|[Aggiorna il tuo account Lite a un account Pagamento a consumo {{site.data.keyword.Bluemix_notm}}](/docs/account/index.html#billableacts) che è configurato con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer).|
|Account Pagamento a consumo precedenti|Gli account Pagamento a consumo che sono stati creati prima che fosse disponibile il collegamento automatico degli account, non venivano forniti con l'acceso al portfolio dell'infrastruttura IBM Cloud (SoftLayer).<p>Se hai un account dell'infrastruttura IBM Cloud (SoftLayer) esistente, non puoi collegarlo a un vecchio account Pagamento a consumo.</p>|Opzione 1: [Crea un nuovo account Pagamento a consumo](/docs/account/index.html#billableacts) che è configurato con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Se scegli questa opzione,
hai due account e fatture {{site.data.keyword.Bluemix_notm}}
separati.<p>Se vuoi continuare a utilizzare il tuo vecchio account Pagamento a consumo per creare i cluster standard, puoi utilizzare il nuovo account Pagamento a consumo per generare una chiave API per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Dovrai quindi impostare la chiave API
per il tuo vecchio account Pagamento a consumo. Per ulteriori informazioni, vedi [Generazione di
una chiave API per i vecchi account Pagamento a consumo e Sottoscrizione](#old_account). Ricorda che le risorse dell'infrastruttura IBM Cloud (SoftLayer) vengono fatturate attraverso il tuo nuovo account Pagamento a consumo.</p></br><p>Opzione 2: se hai già un account dell'infrastruttura IBM Cloud (SoftLayer) esistente che vuoi utilizzare, puoi [impostare le credenziali](cs_cli_reference.html#cs_credentials_set) per il tuo account {{site.data.keyword.Bluemix_notm}}.</p><p>Nota: l'account dell'infrastruttura IBM Cloud (SoftLayer) che utilizzi con il tuo account {{site.data.keyword.Bluemix_notm}} deve essere configurato con le autorizzazioni di Superuser.</p>|
|Account Sottoscrizione|Gli account Sottoscrizione non sono configurati con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer).|Opzione 1: [Crea un nuovo account Pagamento a consumo](/docs/account/index.html#billableacts) che è configurato con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Se scegli questa opzione,
hai due account e fatture {{site.data.keyword.Bluemix_notm}}
separati.<p>Se vuoi continuare a utilizzare il tuo account Sottoscrizione per creare i cluster standard, puoi utilizzare il nuovo account Pagamento a consumo per generare una chiave API per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Dovrai quindi impostare la chiave API
per il tuo account Sottoscrizione. Per ulteriori informazioni, vedi [Generazione di
una chiave API per i vecchi account Pagamento a consumo e Sottoscrizione](#old_account). Ricorda che le risorse dell'infrastruttura IBM Cloud (SoftLayer) vengono fatturate attraverso il tuo nuovo account Pagamento a consumo.</p></br><p>Opzione 2: se hai già un account dell'infrastruttura IBM Cloud (SoftLayer) esistente che vuoi utilizzare, puoi [impostare le credenziali](cs_cli_reference.html#cs_credentials_set) per il tuo account {{site.data.keyword.Bluemix_notm}}.<p>Nota: l'account dell'infrastruttura IBM Cloud (SoftLayer) che utilizzi con il tuo account {{site.data.keyword.Bluemix_notm}} deve essere configurato con le autorizzazioni di Superuser.</p>|
|Account dell'infrastruttura IBM Cloud (SoftLayer) , nessun account {{site.data.keyword.Bluemix_notm}}|Per creare un cluster standard, devi avere un account {{site.data.keyword.Bluemix_notm}}.|<p>[Crea un nuovo account Pagamento a consumo](/docs/account/index.html#billableacts) che è configurato con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Se scegli questa opzione, viene creato per te un account dell'infrastruttura IBM Cloud (SoftLayer). Hai la fatturazione e due account dell'infrastruttura IBM Cloud (SoftLayer) separati.</p>|

<br />


## Generazione di una chiave API dell'infrastruttura IBM Cloud (SoftLayer) da utilizzare con gli account {{site.data.keyword.Bluemix_notm}}
{: #old_account}

Per continuare a utilizzare il tuo vecchio account Pagamento a consumo o Sottoscrizione per creare i cluster standard, genera una chiave API con il tuo nuovo account Pagamento a consumo e impostala per il tuo vecchio account.
{:shortdesc}

Prima di iniziare, crea un account Pagamento a consumo {{site.data.keyword.Bluemix_notm}} che viene configurato automaticamente con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer).

1.  Accedi al [portale dell'infrastruttura IBM Cloud (SoftLayer) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/) utilizzando l'{{site.data.keyword.ibmid}} e la password che hai creato per il tuo nuovo account Pagamento a consumo.
2.  Seleziona **Account** e quindi **Utenti**.
3.  Fai clic su **Genera** per generare una chiave API dell'infrastruttura IBM Cloud (SoftLayer) per il tuo nuovo account Pagamento a consumo.
4.  Copia la chiave API.
5.  Dalla CLI, accedi a {{site.data.keyword.Bluemix_notm}}
utilizzando {{site.data.keyword.ibmid}} e password del tuo vecchio account Pagamento a consumo o
Sottoscrizione.

  ```
  bx login
  ```
  {: pre}

6.  Imposta la chiave API creata in precedenza per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Sostituisci `<API_key>` con la chiave API e `<username>` con {{site.data.keyword.ibmid}} del tuo nuovo account a a Pagamento a consumo.

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  Inizia a [creare i cluster standard](cs_clusters.html#clusters_cli).

**Nota:** per esaminare la tua chiave API dopo averla generata, segui i passi 1 e 2 e quindi, nella
sezione **Chiave API**, fai clic su **Visualizza** per visualizzare la chiave API
per il tuo ID utente.

