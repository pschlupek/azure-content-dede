 <properties
	pageTitle="Verwenden des HDInsight-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Anwendungen, die in HDInsight Spark-Clustern ausgeführt werden | Microsoft Azure"
	description="Erfahren Sie, wie Sie das HDInsight-Plug-In für IntelliJ IDEA zum Remotedebuggen von Anwendungen verwenden, die in HDInsight Spark-Clustern ausgeführt werden."
	services="hdinsight"
	documentationCenter=""
	authors="nitinme"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="nitinme"/>


# Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen im HDInsight Spark-Cluster unter Linux

Dieser Artikel enthält eine Schritt-für-Schritt-Anleitung zur Verwendung des HDInsight-Tools-Plug-Ins für IntelliJ IDEA, um einen Spark-Auftrag im HDInsight Spark-Cluster zu senden und dann über den Desktopcomputer per Remotezugriff zu debuggen. Hierzu müssen Sie die folgenden allgemeinen Schritte ausführen:

1. Erstellen Sie ein Azure Virtual Network vom Typ „Standort zu Standort“ oder „Punkt zu Standort“. Für die Schritte in diesem Dokument wird vorausgesetzt, dass Sie ein Standort-zu-Standort-Netzwerk verwenden.

2. Erstellen Sie einen Spark-Cluster in Azure HDInsight, der Teil des Standort-zu-Standort-Azure Virtual Network ist.

3. Überprüfen Sie die Verbindung zwischen dem Cluster-Hauptknoten und Ihrem Desktop.

4. Erstellen Sie eine Scala-Anwendung in IntelliJ IDEA, und konfigurieren Sie sie für das Remotedebuggen.

5. Stellen Sie die Anwendung bereit, und führen Sie das Debuggen durch.

##Voraussetzungen

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).

* Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
 
* Oracle Java Development Kit. Das Installationspaket finden Sie [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
 
* IntelliJ IDEA. In diesem Artikel wird die Version 15.0.1 verwendet. Das Installationspaket finden Sie [hier](https://www.jetbrains.com/idea/download/).
 
* HDInsight-Tools-Plug-In für IntelliJ IDEA: Das HDInsight-Tools-Plug-In für IntelliJ IDEA ist als Teil des Azure-Toolkits für IntelliJ verfügbar. Anweisungen zum Installieren des Azure-Toolkits finden Sie unter [Installieren des Azure Toolkit für IntelliJ](../azure-toolkit-for-intellij-installation.md).
 
* Beim Ausführen der Spark Scala-Anwendung für das Remotedebuggen auf einem Windows-Computer erhalten Sie ggf. eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben, die aufgrund der unter Windows fehlenden Datei „WinUtils.exe“ ausgelöst wird. Um diesen Fehler zu umgehen, müssen Sie [die ausführbare Datei hier herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\\WinUtils\\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP\_HOME** hinzu, und setzen Sie den Wert der Variablen auf **C\\WinUtils**.

## Schritt 1: Erstellen eines Azure Virtual Network

Führen Sie die Anweisungen unter den unten angegebenen Links aus, um ein Azure Virtual Network zu erstellen, und überprüfen Sie dann die Verbindung zwischen dem Desktop und dem Azure Virtual Network.

* [Erstellen eines VNet mit einer Site-to-Site-VPN-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Erstellen eines VNet mit einer Site-to-Site-VPN-Verbindung per PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung mit einem virtuellen Netzwerk mithilfe von PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## Schritt 2: Erstellen eines HDInsight Spark-Clusters

Sie sollten auch einen Apache Spark-Cluster unter Azure HDInsight erstellen, der Teil des von Ihnen erstellten Azure Virtual Network ist. Verwenden Sie die unter [Erstellen von Linux-basierten Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) verfügbaren Informationen. Wählen Sie als Teil der optionalen Konfiguration das Azure Virtual Network aus, das Sie im vorherigen Schritt erstellt haben.

## Schritt 3: Überprüfen der Verbindung zwischen dem Cluster-Hauptknoten und Ihrem Desktop

1. Rufen Sie die IP-Adresse des Hauptknotens ab. Öffnen Sie die Ambari-Benutzeroberfläche für den Cluster. Klicken Sie auf dem Clusterblatt auf **Dashboard**.

	![IP-Adresse des Hauptknotens ermitteln](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/launch-ambari-ui.png)

2. Klicken Sie in der Ambari-Benutzeroberfläche oben rechts auf **Hosts**.

	![IP-Adresse des Hauptknotens ermitteln](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/ambari-hosts.png)

3. Eine Liste mit Hauptknoten, Workerknoten und Zookeeper-Knoten wird angezeigt. Die Hauptknoten haben das Präfix **hn**. Klicken Sie auf den ersten Hauptknoten.

	![IP-Adresse des Hauptknotens ermitteln](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/cluster-headnodes.png)

4. Kopieren Sie unten auf der sich öffnenden Seite im Feld **Zusammenfassung** die IP-Adresse des Hauptknotens und den Hostnamen.

	![IP-Adresse des Hauptknotens ermitteln](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/headnode-ip-address.png)

5. Fügen Sie die IP-Adresse und den Hostnamen in die Datei **hosts** auf dem Computer ein, über den Sie die Spark-Aufträge ausführen und per Remotezugriff debuggen möchten. Auf diese Weise können Sie mit dem Hauptknoten über die IP-Adresse und den Hostnamen kommunizieren.

	1. Öffnen Sie einen Editor mit erhöhten Rechten. Klicken Sie im Menü „Datei“ auf **Öffnen**, und navigieren Sie zum Speicherort der Datei „hosts“. Auf einem Windows-Computer ist dies `C:\Windows\System32\Drivers\etc\hosts`.

	2. Fügen Sie der Datei **hosts** Folgendes hinzu:

			# For headnode0
			192.xxx.xx.xx hn0-nitinp
			192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

			# For headnode1
			192.xxx.xx.xx hn1-nitinp
			192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net


5. Überprüfen Sie auf dem Computer mit Verbindung zu dem Azure Virtual Network, das vom HDInsight-Cluster verwendet wird, ob sowohl die Hauptknoten als auch der Hostname unter der IP-Adresse per Ping erreichbar sind.

6. Greifen Sie per SSH auf den Cluster-Hauptknoten zu, indem Sie die Anleitung unter [Herstellen einer Verbindung mit einem HDInsight-Cluster per SSH](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster) verwenden. Senden Sie vom Cluster-Hauptknoten aus eine Ping-Nachricht an die IP-Adresse des Desktopcomputers. Es ist ratsam, die Verbindung für beide IP-Adressen zu testen, die dem Computer zugewiesen sind: eine für die Netzwerkverbindung und die andere für das Azure Virtual Network, mit dem der Computer verbunden ist.

7. Wiederholen Sie die Schritte auch für den anderen Hauptknoten.

## Schritt 4: Erstellen einer Spark Scala-Anwendung mit dem HDInsight IntelliJ IDEA-Plug-In und Konfigurieren für das Remotedebuggen

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

	* Wählen Sie im linken Bereich **HDInsight** aus.
	* Wählen Sie im rechten Bereich **Spark on HDInsight (Scala)** aus.
	* Klicken Sie auf **Weiter**.

2. Geben Sie im nächsten Fenster die Projektdetails an.

	* Geben Sie einen Projektnamen und den Projektspeicherort an.
	* Geben Sie für **Project SDK** eine Java-Version höher als 7 an.
	* Klicken Sie für **Scala SDK** auf **Create** (Erstellen) und dann auf **Download**, und wählen Sie die zu verwendende Version von Scala aus. **Verwenden Sie nicht die Version 2.11.x**. In diesem Beispiel wird Version **2.10.6** verwendet.

		![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-version.png)

	* Für **Spark SDK**: Laden Sie das zu verwendende SDK [hier](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunter. Sie können stattdessen auch das [Spark Maven Repository](http://mvnrepository.com/search?q=spark) verwenden. Stellen Sie jedoch sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. (Wenn Sie z. B. Spark Streaming verwenden, müssen Sie sicherstellen, dass Sie den Teil für Spark Streaming installiert haben. Vergewissern Sie sich auch, dass Sie das Repository für Scala 2.10 verwenden. Verwenden Sie nicht das Repository für Scala 2.11.)

		![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-project-details.png)

	* Klicken Sie auf **Fertig stellen**.

3. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Gehen Sie wie folgt vor, um das Artefakt anzuzeigen:

	1. Klicken Sie im Menü **Datei** auf **Projektstruktur**.
	2. Klicken Sie im Dialogfeld **Projektstruktur** auf **Artefakte**, um das Standardartefakt anzuzeigen, das erstellt wird.

		![JAR-Datei erstellen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/default-artifact.png)

	Sie können auch eigene Artefakte erstellen, indem Sie auf das in der Abbildung oben hervorgehobene **+**-Symbol klicken.

4. Klicken Sie im Dialogfeld **Project Structure** auf **Project**. Wenn **Project SDK** auf 1.8 festgelegt ist, stellen Sie sicher, dass **Project language level** auf **7 - Diamonds, ARM, multi-catch, etc** festgelegt ist.

	![Festlegen der Sprache auf Projektebene](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/set-project-language-level.png)

4. Fügen Sie dem Projekt Bibliotheken hinzu. Klicken Sie zum Hinzufügen einer Bibliothek in der Projektstruktur mit der rechten Maustaste auf den Projektnamen, und klicken Sie dann auf **Open Module Settings** (Moduleinstellungen öffnen). Klicken Sie im Dialogfeld **Projektstruktur** im linken Bereich auf **Bibliotheken**, und klicken Sie dann auf das Symbol „(+)“ und die Option **From Maven** (Aus Maven).

	![Bibliothek hinzufügen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/add-library.png)

	Suchen Sie im Dialogfeld **Download Library from Maven Repository** (Bibliothek aus Maven-Repository herunterladen) nach den folgenden Bibliotheken, und fügen Sie sie hinzu:

	* `org.scalatest:scalatest_2.10:2.2.1`
	* `org.apache.hadoop:hadoop-azure:2.7.1`

5. Kopieren Sie `yarn-site.xml` und `core-site.xml` aus dem Cluster-Hauptknoten, und fügen Sie diese Elemente dem Projekt hinzu. Führen Sie die folgenden Befehle aus, um die Dateien zu kopieren: Sie können [Cygwin](https://cygwin.com/install.html) verwenden, um die folgenden `scp`-Befehle zum Kopieren der Dateien aus den Cluster-Hauptknoten auszuführen.

		scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

	Da wir die IP-Adresse und den Hostnamen des Cluster-Hauptknotens der Datei „hosts“ auf dem Desktop bereits hinzugefügt haben, können wir die **scp**-Befehle wie folgt verwenden:

		scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
		scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

	Fügen Sie diese Dateien dem Projekt hinzu, indem Sie sie in den Ordner **/src** der Projektstruktur kopieren, z.B. `<your project directory>\src`.

6. Aktualisieren Sie die Datei `core-site.xml`, um die folgenden Änderungen vorzunehmen.

	1. `core-site.xml` enthält den verschlüsselten Schlüssel für das Speicherkonto, das dem Cluster zugeordnet ist. Ersetzen Sie in der Datei `core-site.xml`, die Sie dem Projekt hinzugefügt haben, den verschlüsselten Schlüssel durch den tatsächlichen Speicherschlüssel, der dem Standardspeicherkonto zugeordnet ist. Weitere Informationen finden Sie unter [Verwalten von Speicherzugriffsschlüsseln](../storage/storage-create-storage-account.md#manage-your-storage-account).

			<property>
	      		<name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
	      		<value>access-key-associated-with-the-account</value>
	    	</property>

	2. Entfernen Sie die folgenden Einträge aus der Datei `core-site.xml`.

			<property>
      			<name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
      			<value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
			</property>

			<property>
		      	<name>fs.azure.shellkeyprovider.script</name>
		      	<value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
		    </property>

			<property>
      			<name>net.topology.script.file.name</name>
      			<value>/etc/hadoop/conf/topology_script.py</value>
    		</property>

	3. Speichern Sie die Datei.

7. Fügen Sie die Hauptklasse (Main) für Ihre Anwendung hinzu. Klicken Sie im **Project Explorer** mit der rechten Maustaste auf **src**, zeigen Sie auf **New** (Neu), und klicken Sie dann auf **Scala class** (Scala-Klasse).

	![Hinzufügen von Quellcode](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code.png)

8. Geben Sie im Dialogfeld **Create New Scala Class** (Neue Scala-Klasse erstellen) einen Namen ein. Wählen Sie **Object** (Objekt) für **Kind** (Variante) aus, und klicken Sie dann auf **OK**.

	![Hinzufügen von Quellcode](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code-object.png)

9. Fügen Sie den folgenden Code in die Datei `MyClusterAppMain.scala` ein. Mit diesem Code wird der Spark-Kontext erstellt und über das `SparkSample`-Objekt eine `executeJob`-Methode gestartet.


		import org.apache.spark.{SparkConf, SparkContext}

		object SparkSampleMain {
		  def main (arg: Array[String]): Unit = {
		    val conf = new SparkConf().setAppName("SparkSample")
		                              .set("spark.hadoop.validateOutputSpecs", "false")
		    val sc = new SparkContext(conf)
		
		    SparkSample.executeJob(sc,
		                           "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
		                           "wasb:///HVACOut")
		  }
		}

10. Wiederholen Sie die obigen Schritte 8 und 9, um ein neues Scala-Objekt mit dem Namen `SparkSample` hinzuzufügen. Fügen Sie dieser Klasse den folgenden Code hinzu. Dieser Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.

		import org.apache.spark.SparkContext
	
		object SparkSample {
		  def executeJob (sc: SparkContext, input: String, output: String): Unit = {
		    val rdd = sc.textFile(input)
		
		    //find the rows which have only one digit in the 7th column in the CSV
		    val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
		
		    val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
		    println(s)
		
		    rdd1.saveAsTextFile(output)
		    //rdd1.collect().foreach(println)
		  }
		
		}

11. Wiederholen Sie die obigen Schritte 8 und 9, um eine neue Klasse mit dem Namen `RemoteClusterDebugging` hinzuzufügen. Mit dieser Klasse wird das Spark-Testframework implementiert, das zum Debuggen von Anwendungen verwendet wird. Fügen Sie der `RemoteClusterDebugging`-Klasse den folgenden Code hinzu.

		import org.apache.spark.{SparkConf, SparkContext}
		import org.scalatest.FunSuite
		
		class RemoteClusterDebugging extends FunSuite {
		
		  test("Remote run") {
		    val conf = new SparkConf().setAppName("SparkSample")
		                              .setMaster("yarn-client")
		                              .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
		                              .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
		                              .setJars(Seq("""C:\WORK\IntelliJApps\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
		                              .set("spark.hadoop.validateOutputSpecs", "false")
		    val sc = new SparkContext(conf)
		
		    SparkSample.executeJob(sc,
		      "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
		      "wasb:///HVACOut")
		  }
		}

	Hierbei sind einige wichtige Punkte zu beachten:
	
	* Stellen Sie für `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")` sicher, dass die Spark-Assembly-JAR-Datei im Clusterspeicher unter dem angegebenen Pfad verfügbar ist.
	* Geben Sie für `setJars` den Speicherort an, an dem die Artefakt-JAR-Datei erstellt wird. Normalerweise ist dies `<Your IntelliJ project directory>\out<project name>_DefaultArtifact\default_artifact.jar`.


11. Klicken Sie in der `RemoteClusterDebugging`-Klasse mit der rechten Maustaste auf das Schlüsselwort `test`, und wählen Sie **Create RemoteClusterDebugging Configuration** (RemoteClusterDebugging-Konfiguration erstellen).

	![Remotekonfiguration erstellen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-remote-config.png)

12. Geben Sie im Dialogfeld einen Namen für die Konfiguration an, und wählen Sie für **Test kind** (Testvariante) die Option **Test name** (Testname) aus. Behalten Sie für alle anderen Werte die Standardeinstellung bei, und klicken Sie auf **Übernehmen** und dann auf **OK**.

	![Remotekonfiguration erstellen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/provide-config-value.png)



13. In der Menüleiste sollte nun die Dropdownliste **Remotetestlauf** für die Konfiguration angezeigt werden.

	![Remotekonfiguration erstellen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/config-run.png)

## Schritt 5: Ausführen der Anwendung im Debugmodus

1. Öffnen Sie im IntelliJ IDEA-Projekt die Datei `SparkSample.scala`, und erstellen Sie neben „val rdd1“ einen Haltepunkt. Wählen Sie im Popupmenü zum Erstellen eines Haltepunkts die Option **line in function executeJob** (Zeile in Funktion executeJob).

	![Haltepunkt hinzufügen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-breakpoint.png)

2. Klicken Sie neben der Dropdownliste **Remotetestlauf** für die Konfiguration auf die Schaltfläche **Debug Run** (Debugausführung), um die Ausführung der Anwendung zu starten.

	![Programm im Debugmodus ausführen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-run-mode.png)

3. Wenn die Programmausführung den Haltepunkt erreicht, sollte im unteren Bereich die Registerkarte **Debugger** angezeigt werden.

	![Programm im Debugmodus ausführen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch.png)

4. Klicken Sie auf das Symbol (**+**), um wie unten gezeigt eine Überwachung hinzuzufügen.

	![Programm im Debugmodus ausführen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable.png)

	Da die Anwendung vor der Erstellung der Variablen `rdd1` unterbrochen wurde, können wir mit dieser Überprüfung sehen, welche ersten fünf Zeilen die Variable `rdd` enthält. Betätigen Sie die **EINGABETASTE**.

	![Programm im Debugmodus ausführen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable-value.png)

	In der obigen Abbildung ist Folgendes dargestellt: Zur Laufzeit können Sie Daten im Terabyte-Bereich abfragen und den Ablauf der Anwendung debuggen. Anhand der Ausgabe in der obigen Abbildung können Sie beispielsweise erkennen, dass die erste Zeile der Ausgabe ein Header ist. Basierend auf diesen Informationen können Sie den Anwendungscode ändern, um die Headerzeile bei Bedarf zu überspringen.

5. Nun können Sie auf das Symbol **Resume Program** (Programm fortsetzen) klicken, um die Ausführung der Anwendung fortzusetzen.

	![Programm im Debugmodus ausführen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-continue-run.png)

6. Wenn die Anwendung erfolgreich ausgeführt wird, sollte eine Ausgabe der folgenden Art angezeigt werden.

	![Programm im Debugmodus ausführen](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-complete.png)

 

## <a name="seealso"></a>Weitere Informationen


* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### Szenarios

* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)

* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Erstellen und Ausführen von Anwendungen

* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Tools und Erweiterungen

* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Use HDInsight Tools Plugin for Eclipse to create Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md) (Verwenden des HDInsight-Tools-Plug-Ins für Eclipse zum Erstellen von Spark-Anwendungen)

* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

<!---HONumber=AcomDC_0706_2016-->