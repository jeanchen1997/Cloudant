---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

<!-- Acrolinx: 2018-05-31 -->

# Esercitazione introduttiva
{: #getting-started-with-cloudant}

In questa esercitazione introduttiva di {{site.data.keyword.cloudantfull}}
usiamo Python per creare un database {{site.data.keyword.cloudant_short_notm}}
e popolare tale database con una semplice raccolta di dati.
{:shortdesc}

Oltre a questa esercitazione, vedi le nostre esercitazioni pratiche che ti aiutano a conoscere meglio {{site.data.keyword.cloudant_short_notm}}. In alternativa, prova una delle esercitazioni dedicate a uno specifico linguaggio:

- [Liberty for Java e {{site.data.keyword.cloudant_short_notm}} ![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/runtimes/liberty/getting-started.html#getting-started-tutorial){:new_window}
- [Node.js e {{site.data.keyword.cloudant_short_notm}} ![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/runtimes/nodejs/getting-started.html#getting-started-tutorial){:new_window}
- [Swift e {{site.data.keyword.cloudant_short_notm}} ![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/runtimes/swift/getting-started.html#getting-started-tutorial){:new_window}

Per ulteriori esercitazioni specifiche per i linguaggi, consulta le informazioni contenute in [Inizia a distribuire la tua prima applicazione![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/){:new_window}. 

<div id="prerequisites"></div>

## Prima di iniziare
{: #prereqs}

Hai bisogno di un account [{{site.data.keyword.cloud}} ![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/registration/){:new_window},
un'istanza del servizio {{site.data.keyword.cloudant_short_notm}} e dei seguenti requisiti Python:

*	Installa la versione più recente del [linguaggio di programmazione Python ![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](https://www.python.org/){:new_window} sul tuo sistema.
	
	Per verificare,
esegui il seguente comando quando richiesto:
	```sh
	python --version
	```
	{:pre}
	
	Vedi un risultato simile al seguente:

	```
	Python 2.7.12
	```
	{:screen}

*	Installa la [libreria Python](libraries/supported.html#python)
	per abilitare le tue applicazioni Python a utilizzare
	{{site.data.keyword.cloudant_short_notm}} su {{site.data.keyword.cloud_notm}}.
	
	Per verificare di avere installato correttamente la libreria client, esegui questo comando in un prompt:
	```sh
	pip freeze
	```
	{:pre}
	
	Vedrai un elenco di tutti i moduli Python installati sul tuo sistema. Esamina l'elenco, cercando una voce {{site.data.keyword.cloudant_short_notm}} simile alla seguente:

	```
	cloudant==2.3.1
	```
	{:screen}
	
	Se il modulo `cloudant` non è installato, installalo utilizzando un comando simile al seguente:
	
	```
	pip install cloudant==2.3.1
	```
	{:pre}

## Passo 1: connetti la tua istanza del servizio {{site.data.keyword.cloudant_short_notm}} su {{site.data.keyword.cloud_notm}}
{: #step-1-connect-to-your-cloudant-nosql-db-service-instance-on-ibm-cloud}

1.	Esegui le istruzioni `import` dei componenti della libreria client {{site.data.keyword.cloudant_short_notm}}
	per abilitare la tua applicazione Python a stabilire una connessione all'istanza del servizio
	{{site.data.keyword.cloudant_short_notm}}.
	```python
	from cloudant.client import Cloudant
	from cloudant.error import CloudantException
	from cloudant.result import Result, ResultByKey
	```
	{: codeblock}

2.  Crea una nuova credenziale del servizio {{site.data.keyword.cloudant_short_notm}}:
  <br>Nella console {{site.data.keyword.cloud_notm}}, apri il dashboard per la tua istanza del servizio.
  <br>Dal menu di navigazione di sinistra, fai clic su `Credenziali del servizio`.
  <br>a. Fai clic sul pulsante `Nuova credenziale`.
  <br>![Crea nuove credenziali del servizio](tutorials/images/img0050.png)
  <br>b. Immetti un nome per la nuova credenziale nella finestra Aggiungi nuova credenziale, come mostrato nella seguente acquisizione di schermo.
  <br>c. (Facoltativo) Aggiungi i parametri di configurazione incorporati.
  <br>d. Fai clic sul pulsante `Aggiungi`.
  <br>![Aggiungi una nuova credenziale del servizio](tutorials/images/img0051.png)
  <br>Le tue credenziali vengono aggiunte alla tabella Credenziali del servizio.
  <br>e. Fai clic su `Visualizza credenziali` in Azioni.
  <br>![Visualizza tutte le credenziali del servizio](tutorials/images/img0052.png)
  <br>Vengono visualizzati i dettagli per le credenziali del servizio:
   <br>![Le credenziali del servizio {{site.data.keyword.cloudant_short_notm}}](tutorials/images/img0009.png)
   
3.	Stabilisci una connessione all'istanza del servizio immettendo il seguente comando:
	Sostituisci le credenziali del servizio dal passo precedente:
	```python
	client = Cloudant("<username>", "<password>", url="<url>")
	client.connect()
	```
	{: codeblock}


## Passo 2: crea un database
{: #step-2-create-a-database}

1. Definisci una variabile nell'applicazione Python:
  ```python
  databaseName = "<yourDatabaseName>"
  ```
  {: codeblock}
  ... dove `<yourDatabaseName>` è il nome che vuoi fornire al tuo database. 

  Il nome database deve iniziare con una lettera e può includere caratteri minuscoli (a-z), numeri (0-9) e uno qualsiasi dei seguenti caratteri `_`, `$`, `(`, `)`, `+`, `-` e `/`.
  {: tip}

2. Crea il database:
  ```python
  myDatabase = client.create_database(databaseName)
  ```
  {: codeblock}

3. Conferma che il database è stato creato correttamente:
  ```python
  if myDatabase.exists():
      print "'{0}' successfully created.\n".format(databaseName)
  ```
  {: codeblock}

## Passo 3: memorizza come documenti una piccola raccolta di dati all'interno del database
{: #step-3-store-a-small-collection-of-data-as-documents-within-the-database}

1. Definisci una raccolta di dati:
  ```python
  sampleData = [
      [1, "one", "boiling", 100],
      [2, "two", "hot", 40],
      [3, "three", "warm", 20],
      [4, "four", "cold", 10],
      [5, "five", "freezing", 0]
    ]
  ```
  {: codeblock}

2. Utilizza il codice Python per 'passare' attraverso i dati e convertirli in documenti JSON.
  Ogni documento viene memorizzato nel database:

  ```python
  # Create documents by using the sample data.
  # Go through each row in the array
  for document in sampleData:
    # Retrieve the fields in each row.
    number = document[0]
    name = document[1]
    description = document[2]
    temperature = document[3]

    # Create a JSON document that represents
    # all the data in the row.
    jsonDocument = {
        "numberField": number,
        "nameField": name,
        "descriptionField": description,
        "temperatureField": temperature
    }

    # Create a document by using the database API.
    newDocument = myDatabase.create_document(jsonDocument)

    # Check that the document exists in the database.
    if newDocument.exists():
        print "Document '{0}' successfully created.".format(number)
  ```
  {: codeblock}

  Nota che eseguiamo un controllo per garantire che ciascun documento venga creato correttamente.
  {: tip}

## Passo 4: recupero di dati tramite query
{: #step-4-retrieving-data-through-queries}

Una piccola raccolta di dati è stata archiviata come documenti nel database.
Puoi effettuare un recupero minimo o completo di tali dati dal database.
Un recupero minimo ottiene i dati di base _relativi_ a un documento.
Un recupero completo include anche i dati _all'interno_ di un documento.

* Per eseguire un richiamo minimo:
  1. Per prima cosa, richiedi un elenco di tutti i documenti nel database.
    ```python
    result_collection = Result(myDatabase.all_docs)
    ```      
    {: codeblock}

    Questo elenco viene restituito in forma di array.

  2. Visualizza il contenuto di un elemento nell'array.
    ```python
    print "Retrieved minimal document:\n{0}\n".format(result_collection[0])
    ```
    {: codeblock}

    Il risultato è simile al seguente esempio:
    
    ```
    [{u'value': {u'rev': u'1-106e76a2612ea13468b2f243ea75c9b1'}, u'id': u'14be111aac74534cf8d390eaa57db888', u'key': u'14be111aac74534cf8d390eaa57db888'}]
    ```
    {:screen}
    
    Il prefisso `u` è un'indicazione che Python sta visualizzando una stringa Unicode.
    {: tip}

    Se ordiniamo un po' l'aspetto, possiamo vedere che i dettagli minimi del documento che abbiamo ottenuto sono equivalenti a questo esempio:
    
    ```json
    [
        {
            "value": {
                "rev": "1-106e76a2612ea13468b2f243ea75c9b1"
            },
            "id": "14be111aac74534cf8d390eaa57db888",
            "key": "14be111aac74534cf8d390eaa57db888"
        }
    ]
    ```
    {: codeblock}

    L'idea che il primo documento archiviato in un database sia sempre il primo documento restituito in un elenco di risultati non si applica sempre ai database NoSQL come {{site.data.keyword.cloudant_short_notm}}.
    {: tip}

* Per eseguire un richiamo completo,
  richiedi un elenco di tutti i documenti nel database e
  specifica che anche il contenuto del documento deve essere restituito
  fornendo l'opzione `include_docs`.
  ```python
  result_collection = Result(myDatabase.all_docs, include_docs=True)
  print "Retrieved full document:\n{0}\n".format(result_collection[0])
  ```
  {: codeblock}
  
  Il risultato è simile al seguente esempio:
  ```
  [{u'value': {u'rev': u'1-7130413a8c7c5f1de5528fe4d373045c'}, u'id': u'0cfc7d902f613d5fdb7b7818e262353b', u'key': u'0cfc7d902f613d5fdb7b7818e262353b', u'doc': {u'temperatureField': 40, u'descriptionField': u'hot', u'numberField': 2, u'nameField': u'two', u'_id': u'0cfc7d902f613d5fdb7b7818e262353b', u'_rev': u'1-7130413a8c7c5f1de5528fe4d373045c'}}]
  ```
  {: screen}
  
  Se ordiniamo un po' l'aspetto, possiamo vedere che i dettagli completi del documento che abbiamo ottenuto sono equivalenti a questo esempio:
  
  ```json
  [
    {
      "value": {
        "rev": "1-7130413a8c7c5f1de5528fe4d373045c
      },
      "id": "0cfc7d902f613d5fdb7b7818e262353b",
      "key": "0cfc7d902f613d5fdb7b7818e262353b",
      "doc": {
        "temperatureField": 40,
        "descriptionField": "hot",
        "numberField": 2,
        "nameField": "two",
        "_id": "0cfc7d902f613d5fdb7b7818e262353b",
        "_rev": "1-7130413a8c7c5f1de5528fe4d373045c"
      }
    }
  ]
  ```
  {: codeblock}

## Passo 5: recupero dei dati attraverso l'endpoint API {{site.data.keyword.cloudant_short_notm}}
{: #step-5-retrieving-data-through-the-cloudant-nosql-db-api-endpoint}

Puoi anche richiedere un elenco di tutti i documenti e il loro contenuto
richiamando l'[endpoint `/_all_docs`](api/database.html#get-documents) {{site.data.keyword.cloudant_short_notm}}.

1. Identifica l'endpoint da contattare ed eventuali parametri da fornire insieme alla chiamata:
  ```python
  end_point = '{0}/{1}'.format("<url>", databaseName + "/_all_docs")
  params = {'include_docs': 'true'}
  ```
  {: codeblock}
  ... dove `<url>` è il valore URL indicato nelle credenziali del servizio che hai trovato nel passo 1.

2. Invia la richiesta all'istanza di servizio e visualizza i risultati:
  ```python
  response = client.r_session.get(end_point, params=params)
  print "{0}\n".format(response.json())
  ```
  {: codeblock}

  Il risultato è simile al seguente esempio _abbreviato_:
  
  ```
  {u'rows': [{u'value': {u'rev': u'1-6d8cb5905316bf3dbe4075f30daa9f59'}, u'id': u'0532feb6fd6180d79b842d871316c444', u'key': u'0532feb6fd6180d79b842d871316c444', u'doc': {u'temperatureField': 20, u'descriptionField': u'warm', u'numberField': 3, u'nameField': u'three', u'_id': u'0532feb6fd6180d79b842d871316c444', u'_rev': u'1-6d8cb5905316bf3dbe4075f30daa9f59'}}, ... , {u'value': {u'rev': u'1-3f61736fa96473d358365ce1665e3d97'}, u'id': u'db396f77bbe12a567b09177b4accbdbc', u'key': u'db396f77bbe12a567b09177b4accbdbc', u'doc': {u'temperatureField': 0, u'descriptionField': u'freezing', u'numberField': 5, u'nameField': u'five', u'_id': u'db396f77bbe12a567b09177b4accbdbc', u'_rev': u'1-3f61736fa96473d358365ce1665e3d97'}}], u'total_rows': 5, u'offset': 0}
  ```
  {:screen}
  
  Possiamo ordinare un po' l'aspetto e vedere che i dettagli _abbreviati_ che abbiamo ottenuto sono simili al seguente esempio:
  
  ```json
  {
      "rows": [
          {
              "value": {
                "rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              },
              "id": "0532feb6fd6180d79b842d871316c444",
              "key": "0532feb6fd6180d79b842d871316c444",
              "doc": {
                  "temperatureField": 20,
                  "descriptionField": "warm",
                  "numberField": 3,
                  "nameField": "three",
                  "_id": "0532feb6fd6180d79b842d871316c444",
                  "_rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              }
          },
          ...
          {
              "value":
              {
                "rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              },
              "id": "db396f77bbe12a567b09177b4accbdbc",
              "key": "db396f77bbe12a567b09177b4accbdbc",
              "doc": {
                  "temperatureField": 0,
                  "descriptionField": "freezing",
                  "numberField": 5,
                  "nameField": "five",
                  "_id": "db396f77bbe12a567b09177b4accbdbc",
                  "_rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              }
          }
      ],
      "total_rows": 5,
      "offset": 0
  }
  ```
  {: codeblock}

## Passo 6: elimina il database
{: #step-6-delete-the-database}

Una volta terminato di utilizzare il database,
è possibile eliminarlo.

```python
try :
    client.delete_database(databaseName)
except CloudantException:
    print "There was a problem deleting '{0}'.\n".format(databaseName)
else:
    print "'{0}' successfully deleted.\n".format(databaseName)
```
{: codeblock}

Abbiamo incluso della gestione degli errori di base per mostrarti come risolvere potenziali problemi e come occuparsene.

## Passo 7: chiudi la connessione all'istanza del servizio
{: #step-7-close-the-connection-to-the-service-instance}

Il passo finale consiste nel disconnettere l'applicazione client Python dall'istanza del servizio:

```python
client.disconnect()
```
{: codeblock}

## Passi successivi
{: #next-steps}

Per ulteriori informazioni su tutte le offerte {{site.data.keyword.cloudant_short_notm}},
consulta il sito principale di [{{site.data.keyword.cloudant_short_notm}} ![Icona link esterno](images/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/analytics/us/en/technology/cloud-data-services/cloudant/){:new_window}.

Per ulteriori dettagli ed esercitazioni su concetti, attività e tecniche di {{site.data.keyword.cloudant_short_notm}},
vedi la [documentazione di {{site.data.keyword.cloudant_short_notm}}](cloudant.html).

## Appendice: elenco completo di codice Python
{: #appendix-complete-python-code-listing}

L'elenco completo di codice Python è il seguente. 
Ricordati di sostituire i valori `<username>`,
`<password>`,
e `<url>` con le tue credenziali del servizio.
Allo stesso modo,
sostituisci il valore `<yourDatabaseName>` con il nome del tuo database.

```python
from cloudant.client import Cloudant
from cloudant.error import CloudantException
from cloudant.result import Result, ResultByKey

client = Cloudant("<username>", "<password>", url="<url>")
client.connect()

databaseName = "<yourDatabaseName>"

myDatabase = client.create_database(databaseName)

if myDatabase.exists():
    print "'{0}' successfully created.\n".format(databaseName)

sampleData = [
    [1, "one", "boiling", 100],
    [2, "two", "hot", 40],
    [3, "three", "warm", 20],
    [4, "four", "cold", 10],
    [5, "five", "freezing", 0]
]

# Create documents using the sample data.
# Go through each row in the array
  for document in sampleData:
    # Retrieve the fields in each row.
    number = document[0]
    name = document[1]
    description = document[2]
    temperature = document[3]

    # Create a JSON document that represents
    # all the data in the row.
    jsonDocument = {
        "numberField": number,
        "nameField": name,
        "descriptionField": description,
        "temperatureField": temperature
    }

    # Create a document using the Database API.
    newDocument = myDatabase.create_document(jsonDocument)

    # Check that the document exists in the database.
    if newDocument.exists():
        print "Document '{0}' successfully created.".format(number)

result_collection = Result(myDatabase.all_docs)

print "Retrieved minimal document:\n{0}\n".format(result_collection[0])

result_collection = Result(myDatabase.all_docs, include_docs=True)
print "Retrieved full document:\n{0}\n".format(result_collection[0])

end_point = '{0}/{1}'.format("<url>", databaseName + "/_all_docs")
params = {'include_docs': 'true'}
response = client.r_session.get(end_point, params=params)
print "{0}\n".format(response.json())


try :
    client.delete_database(databaseName)
except CloudantException:
    print "There was a problem deleting '{0}'.\n".format(databaseName)
else:
    print "'{0}' successfully deleted.\n".format(databaseName)

client.disconnect()

```
{: codeblock}
