<properties 
	pageTitle="Kopieren/Verschieben von Azure-Blobdatasets | Azure Data Factory" 
	description="Hier erfahren Sie, wie Sie Blobdaten in Azure Data Factory kopieren. Verwenden Sie unser Beispiel zum Kopieren von Daten aus Azure-Blobspeicher in die Azure SQL-Datenbank (und umgekehrt)." 
    keywords="Blobdaten, Azure-Blobkopie"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/06/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten in einen und aus einem Azure-Blob mithilfe von Azure Data Factory
In diesem Artikel erfahren Sie, wie Sie Daten mithilfe der Kopieraktivität von Azure Data Factory in einen und aus einem Azure-Blob verschieben, indem Sie Blobdaten aus einem anderen Datenspeicher heranziehen. Dieser Artikel baut auf dem Artikel zu Datenverschiebungsaktivitäten auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und den unterstützten Datenspeicherkombinationen bietet.

> [AZURE.NOTE] Dieser Azure-Blobconnector unterstützt derzeit nur Kopiervorgänge an/aus Blockblobs. Darüber hinaus unterstützt er sowohl Azure Storage für allgemeine Zwecke als auch Blob Storage (Hot/Cool).

## Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten in Azure-Blobspeicher und daraus kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder über [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen Ihnen das Kopieren von Daten aus Azure-Blobspeicher in die Azure SQL-Datenbank (und umgekehrt). Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.

## Beispiel: Kopieren von Daten aus einem Azure-Blob in Azure SQL
 

Das nachstehende Beispiel zeigt Folgendes:

1.	Einen verknüpften Dienst des Typs [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)
2.	Einen verknüpften Dienst des Typs [AzureStorage](#azure-storage-linked-service-properties)
3.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](#azure-blob-dataset-type-properties)
4.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)
4.	Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](#azure-blob-copy-activity-type-properties) und [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties) verwendet

Im Beispiel werden Daten, die zu einer Zeitreihe aus einem Azure-Blob gehören, stündlich in eine Tabelle in einer Azure SQL-Datenbank kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL verknüpfter Dienst:**

	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**Mit Azure Storage verknüpfter Dienst:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

Azure Data Factory unterstützt zwei Arten von mit Azure Storage verknüpften Diensten: **AzureStorage** und **AzureStorageSas**. Für den ersten geben Sie die Verbindungszeichenfolge an, die den Kontoschlüssel enthält. Für den zweiten geben Sie den SAS-URI (Shared Access Signature) an. Weitere Informationen finden Sie unter [Verknüpfte Dienste](#linked-services).

**Azure-Blob-Eingabedataset:**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad verwendet die Bestandteile Jahr, Monat und Tag der Startzeit, und der Dateiname verwendet die Stunde der Startzeit. Die Festlegung von "external" auf "true" teilt dem Data Factory-Dienst mit, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

	{
	  "name": "AzureBlobInput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
	      "fileName": "{Hour}.csv",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "rowDelimiter": "\n"
	      }
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Azure SQL-Ausgabedataset:**

Das Beispiel kopiert Daten in eine Tabelle namens "MyTable" in einer Azure SQL-Datenbank. Sie sollten die Tabelle in der Azure SQL-Datenbank mit der gleichen Anzahl von Spalten erstellen, die Sie in der Blob-CSV-Datei erwarten. Neue Zeilen werden der Tabelle stündlich hinzugefügt.

	{
	  "name": "AzureSqlOutput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyOutputTable"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **SqlSink** festgelegt.

	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2014-06-01T18:00:00",
	    "end":"2014-06-01T19:00:00",
	    "description":"pipeline with copy activity",
	    "activities":[  
	      {
	        "name": "AzureBlobtoSQL",
	        "description": "Copy Activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureBlobInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureSqlOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource"
	          },
	          "sink": {
	            "type": "SqlSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	      ]
	   }
	}

## Beispiel: Kopieren von Daten aus Azure SQL in ein Azure-Blob
Das nachstehende Beispiel zeigt Folgendes:

1.	Einen verknüpften Dienst des Typs [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)
2.	Einen verknüpften Dienst des Typs [AzureStorage](#azure-storage-linked-service-properties)
3.	Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)
4.	Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](#azure-blob-dataset-type-properties)
4.	Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [SqlSource](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties) und [BlobSink](#azure-blob-copy-activity-type-properties) verwendet


Im Beispiel werden Daten, die zu einer Zeitreihe gehören, stündlich aus einer Tabelle in einer Azure SQL-Datenbank in ein Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL verknüpfter Dienst:**

	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**Mit Azure Storage verknüpfter Dienst:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

Azure Data Factory unterstützt zwei Arten von mit Azure Storage verknüpften Diensten: **AzureStorage** und **AzureStorageSas**. Für den ersten geben Sie die Verbindungszeichenfolge an, die den Kontoschlüssel enthält. Für den zweiten geben Sie den SAS-URI (Shared Access Signature) an. Weitere Informationen finden Sie unter [Verknüpfte Dienste](#linked-services).


**Azure SQL-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in Azure SQL erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält.

Durch Festlegen von "external" auf "true" und Angeben der Richtlinie "externalData" wird dem Azure Data Factory-Dienst angegeben, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

	{
	  "name": "AzureSqlInput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyTable"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}


**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

	
	{
	  "name": "AzureBlobOutput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": "\t",
	        "rowDelimiter": "\n"
	      }
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **SqlReaderQuery**-Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.


	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    	"start":"2014-06-01T18:00:00",
	    	"end":"2014-06-01T19:00:00",
	    	"description":"pipeline for copy activity",
	    	"activities":[  
	      		{
	        		"name": "AzureSQLtoBlob",
		    	    "description": "copy activity",
		    	    "type": "Copy",
		    	    "inputs": [
		    	      {
		    	        "name": "AzureSQLInput"
		    	      }
		    	    ],
		    	    "outputs": [
		    	      {
		    	        "name": "AzureBlobOutput"
		    	      }
		    	    ],
		    	    "typeProperties": {
		    	    	"source": {
		            		"type": "SqlSource",
			            	"SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
		          		},
		          		"sink": {
		            		"type": "BlobSink"
		          		}
		        	},
		       		"scheduler": {
		          		"frequency": "Hour",
		          		"interval": 1
		        	},
		        	"policy": {
		          		"concurrency": 1,
		          		"executionPriorityOrder": "OldestFirst",
		          		"retry": 0,
		          		"timeout": "01:00:00"
		        	}
		      	}
		     ]
		}
	}

## Verknüpfte Dienste
Es gibt zwei Arten von verknüpften Diensten, die Sie verwenden können, um einen Azure-Blobspeicher mit einer Azure Data Factory zu verknüpfen: **AzureStorage** und **AzureStorageSas**. Der mit Azure Storage verknüpfte Dienst bietet der Data Factory einen globalen Zugriff auf Azure-Speicher. Dagegen bietet der mit Azure Storage SAS (Shared Access Signature) verknüpfte Dienst der Data Factory einen eingeschränkten bzw. zeitgebundenen Zugriff auf Azure-Speicher. Es gibt keine weitere Unterschiede zwischen diesen beiden verknüpften Diensten. Wählen Sie den verknüpften Dienst, der Ihren Anforderungen entspricht. Die folgenden Abschnitte bieten weitere Informationen zu diesen beiden verknüpften Diensten.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## Eigenschaften des Dataset-Typs "Azure-Blob"

Eine vollständige Liste der JSON-Abschnitte und -Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie "structure", "availability" und "policy" des JSON-Codes eines Datasets sind bei allen Typen von Datasets (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort, Format usw. der Daten im Datenspeicher. Der Abschnitt "typeProperties" für ein Dataset vom Typ **AzureBlob** hat die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| folderPath | Der Pfad zum Container und Ordner im Blobspeicher. Beispiel: myblobcontainer\\myblobfolder\\ | Ja |
| fileName | Der Name des Blobs. fileName ist optional, wobei die Groß- und Kleinschreibung beachtet werden muss.<br/><br/>Wenn Sie einen Dateinamen angeben, funktioniert die Aktivität (einschließlich Kopieren) für das jeweilige Blob.<br/><br/>Wenn fileName nicht angegeben ist, werden alle Blobs in folderPath für das Eingabedataset kopiert.<br/><br/>Wenn fileName für ein Ausgabedataset nicht angegeben ist, hat der Name der generierten Datei folgendes Format: Data.<GUID>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt). | Nein |
| partitionedBy | "partitionedBy" ist eine optionale Eigenschaft. "partitionedBy" kann genutzt werden, um einen dynamischen Wert für "folderPath" oder "fileName" für Zeitreihendaten anzugeben. Beispiel: "folderPath" kann für jedes stündliche Datenaufkommen parametrisiert werden. Im Abschnitt [Nutzen der Eigenschaft "partitionedBy"](#Leveraging-partitionedBy-property) unten finden Sie Details und Beispiele. | Nein
| Format | Folgende Formattypen werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat** und **OrcFormat**. Legen Sie die **type**-Eigenschaft unter „Format“ auf einen dieser Werte fest. Weitere Informationen finden Sie in den Abschnitten [Angeben von TextFormat](#specifying-textformat), [Angeben von AvroFormat](#specifying-avroformat), [Angeben von JsonFormat](#specifying-jsonformat) und [Angeben von OrcFormat](#specifying-orcformat). Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.| Nein
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Unterstützte Typen: **GZip**, **Deflate** und **BZip2**. Unterstützte Komprimierungsgrade: **Optimal** und **Schnellste**. Beachten Sie, dass für Daten mit **AvroFormat** oder **OrcFormat** derzeit keine Komprimierungseinstellungen unterstützt werden. Weitere Einzelheiten finden Sie im Abschnitt [Komprimierungsunterstützung](#compression-support). | Nein |

### Nutzen der Eigenschaft "partitionedBy"
Wie bereits erwähnt, können Sie die dynamischen Werte "folderPath" und "fileName" für Zeitreihendaten mit dem Abschnitt **partitionedBy**, mit Data Factory-Makros und mit den Systemvariablen "SliceStart" und "SliceEnd" angeben, die die Start- und Endzeit für einen bestimmten Slice festlegen.

Weitere Informationen über Data Factory-Systemvariablen und -Funktionen, die Sie im Abschnitt "partitionedBy" verwenden können, finden Sie unter [Data Factory-Systemvariablen](data-factory-scheduling-and-execution.md#data-factory-system-variables) und [Referenz zu Data Factory-Funktionen](data-factory-scheduling-and-execution.md#data-factory-functions-reference).

In den Artikeln [Erstellen von Datasets](data-factory-create-datasets.md) und [Planung und Ausführung](data-factory-scheduling-and-execution.md) finden Sie weitere Details zu Zeitreihen-Datasets, Planung und Slices.

#### Beispiel 1

	"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
	"partitionedBy": 
	[
	    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
	],

Im obigen Beispiel wird {Slice} durch den Wert der Data Factory-Systemvariablen "SliceStart" ersetzt, die im Format (JJJJMMTTHH) angegeben wird. "SliceStart" bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### Beispiel 2

	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
	"fileName": "{Hour}.csv",
	"partitionedBy": 
	 [
	    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
	    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
	    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
	    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
	],

Im Beispiel oben werden Jahr, Monat, Tag und Uhrzeit von "SliceStart" in separate Variablen extrahiert, die von den Eigenschaften "folderPath" und "fileName" verwendet werden.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]  
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## Eigenschaften des Azure-Blob-Kopieraktivitätstyps  
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen, verschiedene Richtlinien usw. sind für alle Arten von Aktivitäten verfügbar.

Im Abschnitt "typeProperties" der Aktivität verfügbare Eigenschaften variieren hingegen bei jedem Aktivitätstyp. Bei der Kopieraktivität variieren sie je nach Typ der Quellen und Senken.

**BlobSource** unterstützt die folgenden Eigenschaften im Abschnitt **typeProperties**:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- | 
| treatEmptyAsNull | Gibt an, ob Null oder eine leere Zeichenfolge als NULL-Wert behandelt wird. <br/><br/>Hinweis: Wenn die Eigenschaft **quoteChar** angegeben ist, kann eine in Anführungszeichen gesetzte leere Zeichenfolge mit dieser Eigenschaft auch als NULL-Wert behandelt werden. | TRUE (Standardwert) <br/>FALSE | Nein |
| skipHeaderLineCount | Gibt an, wie viele Zeilen übersprungen werden müssen. Nur anwendbar, wenn das Eingabedataset **TextFormat** verwendet. | Ganzzahl zwischen "0" und "Max". | Nein | 
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. | True (Standardwert), False | Nein | 


**BlobSink** unterstützt die folgenden Eigenschaften im Abschnitt **typeProperties**:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| blobWriterAddHeader | Gibt an, ob der Header der Spaltendefinitionen hinzugefügt werden soll. | TRUE<br/>FALSE (Standard) | Nein |
| copyBehavior | Definiert das Verhalten beim Kopieren, wenn die Quelle "BlobSource" oder "FileSystem" ist. | **PreserveHierarchy**: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht also dem relativen Pfad der Zieldatei zum Zielordner.<br/><br/>**FlattenHierarchy**: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Für die Zieldateien wird ein automatisch ein Name erzeugt. <br/><br/>**MergeFiles** (Standardwert): Führt alle Dateien aus dem Quellordner in einer Datei zusammen. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. | Nein |

### Beispiele für "recursive" und "copyBehavior"
Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von rekursiven und CopyBehavior-Werten.

recursive | copyBehavior | Resultierendes Verhalten
--------- | ------------ | --------
true | preserveHierarchy | Für den Quellordner „Ordner1“ mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ die gleiche Struktur wie die Quelle <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.  
true | flattenHierarchy | Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei5
true | mergeFiles | Für einen Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Namen zusammengeführt.
false | preserveHierarchy | Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht ausgewählt.
false | flattenHierarchy | Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierter Name für Datei2<br/><br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht ausgewählt.
false | mergeFiles | Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>hat der Zielordner „Ordner1“ folgende Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 werden zu einer Datei mit einem automatisch generierten Namen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht ausgewählt.

  


[AZURE.INCLUDE [data-factory-structure-for-rectangular-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## Leistung und Optimierung  
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

<!---HONumber=AcomDC_0720_2016-->