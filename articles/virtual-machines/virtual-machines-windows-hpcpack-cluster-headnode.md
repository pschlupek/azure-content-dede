<properties
 pageTitle="Erstellen eines HPC Pack-Hauptknotens in einem virtuellen Azure-Computer | Microsoft Azure"
 description="Sie erfahren, wie Sie das Azure-Portal und das Ressourcen-Manager-Bereitstellungsmodell verwenden, um einen Microsoft HPC Pack-Hauptknoten in einem virtuellen Azure-Computer zu erstellen."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="05/19/2016"
 ms.author="danlep"/>

# Erstellen des Hauptknotens eines HPC Pack-Clusters auf einem virtuellen Azure-Computer mit einem Marketplace-Image


Verwenden Sie ein [Microsoft HPC Pack-Image für einen virtuellen Computer](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) aus Azure Marketplace, um den Hauptknoten eines HPC-Clusters mithilfe des Azure-Portals zu erstellen. Das HPC Pack-VM-Image basiert auf Windows Server 2012 R2 Datacenter mit vorinstalliertem HPC Pack 2012 R2 Update 3. Verwenden Sie diesen Hauptknoten für eine Proof of Concept-Bereitstellung von HPC Pack in Azure. Sie können dem Cluster dann Computeknoten zum Ausführen von HPC-Workloads hinzufügen.



>[AZURE.TIP]Für eine Bereitstellung eines vollständigen HPC Pack-Clusters in Azure, der den Hauptknoten und die Computeknoten enthält, sollten Sie eine automatisierte Methode verwenden, z.B. das [HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md), oder eine Resource Manager-Vorlage wie die Vorlage [HPC Pack cluster for Windows workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) (HPC Pack-Cluster für Windows-Workloads). Zusätzliche Vorlagen finden Sie unter [HPC Pack cluster options in Azure](virtual-machines-windows-hpcpack-cluster-options.md) (HPC Pack-Clusteroptionen in Azure).


## Überlegungen zur Planung

Wie in der folgenden Abbildung gezeigt, stellen Sie den Hauptknoten in einer Active Directory-Domäne in einem virtuellen Azure-Netzwerk bereit.

![HPC Pack-Hauptknoten][headnode]

* **Active Directory-Domäne** – Der HPC Pack-Hauptknoten muss einer Active Directory-Domäne in Azure beigetreten sein, bevor Sie die HPC-Dienste auf dem virtuellen Computer starten. Wie in diesem Artikel gezeigt, können Sie die VM, die Sie für den Hauptknoten als Domänencontroller erstellen, für eine Proof of Concept-Bereitstellung höher stufen, bevor Sie die HPC-Dienste starten. Eine weitere Möglichkeit ist das Bereitstellen eines separaten Domänencontrollers und einer Gesamtstruktur in Azure, der der virtuelle Hauptknotencomputer beitritt.

* **Virtuelles Azure-Netzwerk** – Wie in diesem Artikel gezeigt, geben Sie ein virtuelles Azure-Netzwerk an oder erstellen es, wenn Sie den HPC Pack-Hauptknoten mit dem Resource Manager-Bereitstellungsmodell im Azure-Portal bereitstellen. Sie müssen das virtuelle Netzwerk später verwenden, um Clustercomputeknoten-VMs dem HPC-Cluster hinzuzufügen, oder wenn Sie den Hauptknoten einer vorhandenen Active Directory-Domäne beitreten lassen.

    
## Schritte zum Erstellen des Hauptknotens

Dies sind allgemeine Schritte zum Erstellen einer Azure-VM für den HPC Pack-Hauptknoten mit dem Ressourcen-Manager-Bereitstellungsmodell im Azure-Portal.


1. Wenn Sie eine neue Active Directory-Gesamtstruktur in Azure mit separaten Domänencontroller-VMs erstellen möchten, ist die Verwendung einer [Resource Manager-Vorlage](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) eine Möglichkeit. Bei einer einfachen Machbarkeitsstudien-Bereitstellung von HPC Pack können Sie diesen Schritt auslassen. Stattdessen konfigurieren Sie die virtuellen Hauptknotencomputer selbst als Domänencontroller, wie weiter unten in diesem Artikel beschrieben.
    
2. Wählen Sie zum Erstellen des HPC Pack-Hauptknotens im [Azure-Portal](https://portal.azure.com) das HPC Pack 2012 R2-Image aus dem Azure Marketplace. Klicken Sie hierzu auf **Neu**, und suchen Sie im Marketplace das **HPC Pack**. Wählen Sie das Image **HPC Pack 2012 R2 unter Windows Server 2012 R2**.

3. Wählen Sie auf der Seite **HPC Pack 2012 R2 unter Windows Server 2012 R2** das **Resource Manager**-Bereitstellungsmodell, und klicken Sie dann auf **Erstellen**.

    ![HPC Pack-Image][marketplace]

4. Verwenden Sie das Portal, um die Einstellungen zu konfigurieren und die VM zu erstellen. Wenn Sie mit Azure noch nicht vertraut sind, folgen Sie den Schritten im Tutorial [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](virtual-machines-windows-hero-tutorial.md). Für eine Machbarkeitsstudien-Bereitstellung können Sie in der Regel viele Standardeinstellungen oder empfohlene Einstellungen übernehmen.

    **Überlegungen zu virtuellen Netzwerken**

   * Wenn Sie den Hauptknoten einer vorhandenen Active Directory-Domäne in Azure hinzufügen möchten, stellen Sie sicher, dass Sie beim Erstellen der virtuellen Hauptknotencomputer das virtuelle Netzwerk dieser Domäne angeben.
   
   * Wenn Sie ein neues virtuelles Netzwerk erstellen möchten, geben Sie in **Einstellungen** einen privaten Netzwerkadressbereich (z.B. 10.0.0.0/16) und einen Subnetzadressbereich (z.B. 10.0.0.0/24) an.
    
4. Wenn der virtuelle Computer, den Sie erstellt haben, ausgeführt wird, [stellen Sie über Remotedesktop die Verbindung mit dem virtuellen Computer her](virtual-machines-windows-connect-logon.md). 

5. Führen Sie den Beitritt der VM zu einer vorhandenen Gesamtstruktur aus, oder erstellen Sie auf der VM selbst eine neue Domänengesamtstruktur.

    * Wenn Sie den virtuellen Computer in einem virtuellen Azure-Netzwerk mit einer vorhandenen Domänengesamtstruktur erstellt haben, verwenden Sie den standardmäßigen Server-Manager oder Windows PowerShell-Tools, um seinen Beitritt zur Domänengesamtstruktur auszuführen. Führen Sie anschließend einen Neustart durch.

    * Wenn Sie die VM in einem neuen virtuellen Netzwerk ohne vorhandene Domänengesamtstruktur erstellt haben, stufen Sie die VM zum Domänencontroller hoch. Verwenden Sie hierzu den standardmäßigen Server-Manager oder Windows PowerShell-Tools zum Installieren und Konfigurieren der Active Directory-Domänendienste-Rolle. Ausführliche Schritte finden Sie unter [Installieren einer neuen Windows Server 2012 Active Directory-Gesamtstruktur](https://technet.microsoft.com/library/jj574166.aspx).

5. Wenn der virtuelle Computer ausgeführt wird und Mitglied einer Active Directory-Gesamtstruktur ist, starten Sie die HPC Pack-Dienste auf dem Hauptknoten. Gehen Sie dazu folgendermaßen vor:

    a. Stellen Sie eine Verbindung mit dem virtuellen Computer her, indem Sie ein Domänenkonto verwenden, das Mitglied der lokalen Administratorgruppe ist. Nutzen Sie beispielsweise das Administratorkonto, das Sie beim Erstellen des virtuellen Hauptknotencomputers eingerichtet haben.

    b. Starten Sie zur Durchführung einer Standardkonfiguration des Hauptknotens Windows PowerShell als Administrator, und geben Sie Folgendes ein:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Es kann mehrere Minuten dauern, bis die HPC Pack-Dienste starten.

    Geben Sie `get-help HPCHNPrepare.ps1` ein, um weitere Konfigurationsoptionen für den Hauptknoten anzuzeigen.


## Nächste Schritte

* Sie können jetzt mit dem Hauptknoten Ihres HPC Pack-Clusters arbeiten. Starten Sie z.B. den HPC-Cluster-Manager, und führen Sie die [Aufgabenliste für die Bereitstellung](https://technet.microsoft.com/library/jj884141.aspx) aus.
* Fügen Sie [Azure-Burstknoten](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) in einem Clouddienst hinzu, um die bedarfsgesteuerte Clustercomputekapazität heraufzusetzen. 

* Führen Sie eine Test-Workload auf dem Cluster aus. Ein Beispiel hierzu finden Sie im [Leitfaden für die ersten Schritte](https://technet.microsoft.com/library/jj884144) von HPC Pack.

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png

<!---HONumber=AcomDC_0525_2016-->