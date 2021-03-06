<properties
	pageTitle="Erstellen und Hochladen einer Ubuntu-Linux-VHD in Azure"
	description="Erfahren Sie, wie Sie eine virtuelle Azure-Festplatte (Virtual Hard Disk, VHD) erstellen und hochladen, die ein Ubuntu-Linux-Betriebssystem enthält."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="szarkos"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/09/2016"
	ms.author="szark"/>

# Vorbereiten eines virtuellen Ubuntu-Computers für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Offizielle Ubuntu-Cloud-Images
Ubuntu veröffentlicht jetzt auf [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) offizielle Azure-VHDs zum Herunterladen. Wenn Sie Ihr eigenes spezialisiertes Ubuntu-Image für Azure erstellen müssen, empfiehlt es sich, mit diesen bekannten, funktionierenden VHDs zu beginnen und sie nach Bedarf anzupassen statt das manuelle Verfahren unten anzuwenden.


## Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie bereits ein Ubuntu-Linux-Betriebssystem auf einer virtuellen Festplatte installiert haben. Sie können VHD-Dateien mit unterschiedlichen Tools erstellen, beispielsweise mit einer Virtualisierungslösung wie Hyper-V. Anweisungen hierzu finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

**Installationshinweise zu Ubuntu**

- Beachten Sie auch die [allgemeinen Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes). Dort erhalten Sie weitere Tipps zur Vorbereitung von Linux für Azure.
- Das VHDX-Format wird in Azure noch nicht unterstützt, dafür jedoch **virtuelle Festplatten mit fester Größe**. Sie können den Datenträger mit dem Hyper-V-Manager oder dem convert-vhd-Cmdlet in das VHD-Format konvertieren.
- Beim Installieren des Linux-Systems wird empfohlen, anstelle von LVM (bei vielen Installationen oftmals voreingestellt) die Standardpartitionen zu verwenden. Dadurch lässt sich vermeiden, dass ein LVM-Namenskonflikt mit geklonten virtuellen Computern auftritt, besonders dann, wenn ein BS-Datenträger zu Fehlerbehebungszwecken mit einem anderen virtuellen Computer verbunden wird. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträgern verwendet werden.
- Konfigurieren Sie keine SWAP-Partition auf einem Betriebssystemdatenträger. Der Linux-Agent kann konfiguriert werden, eine Auslagerungsdatei auf dem temporären Ressourcendatenträger zu erstellen. Weitere Informationen dazu finden Sie in den folgenden Schritten.
- Alle virtuellen Festplatten müssen eine Größe aufweisen, die ein Vielfaches von 1 MB ist.


## Manuelle Schritte

> [AZURE.NOTE] Ziehen Sie vor dem Erstellen Ihres eigenen benutzerdefinierten Ubuntu-Images für Azure stattdessen die Verwendung eines Images von [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) in Betracht.


1. Wählen Sie den virtuellen Computer im mittleren Fensterbereich des Hyper-V-Managers.

2. Klicken Sie auf **Verbinden**, um das Fenster für den virtuellen Computer zu öffnen.

3.	Ersetzen Sie die aktuellen Repositorys im Image zum Verwenden der Azure-Repositorys von Ubuntu. Die Schritte können je nach Ubuntu-Version geringfügig abweichen.

	Vor der Bearbeitung von "/etc/apt/sources.list" sollten Sie eine Sicherung vornehmen:

		# sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

	Ubuntu 12.04:

		# sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
		# sudo apt-get update

	Ubuntu 14.04:

		# sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
		# sudo apt-get update

4. Die Ubuntu-Azure-Images folgen jetzt dem *HardWare Enablement*-Kernel (HWE). Aktualisieren Sie das Betriebssystem auf den neuesten Kernel, indem Sie die folgenden Befehle ausführen:

	Ubuntu 12.04:

		# sudo apt-get update
		# sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
		# sudo apt-get install hv-kvp-daemon-init
		(recommended) sudo apt-get dist-upgrade

		# sudo reboot

	Ubuntu 14.04:

		# sudo apt-get update
		# sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
		# sudo apt-get install hv-kvp-daemon-init
		(recommended) sudo apt-get dist-upgrade

		# sudo reboot

5.	(optional) Wenn das Ubuntu-System ein Fehler feststellt und neu gestartet wird, wartet es oftmals an der Grub-Starteingabeaufforderung auf eine Benutzereingabe, wodurch das entsprechende Starten des Systems verhindert wird. Um dies zu unterbinden, müssen Sie die folgenden Schritte ausführen:

	a) Öffnen Sie die Datei /etc/grub.d/00\_header.

	b) Suchen Sie in der **make\_timeout()**-Funktion **if ["\\${recordfail}" = 1 ]; then**

	c) Ändern Sie die Anweisung unterhalb dieser Zeile in **set timeout=5**.

	d) Führen Sie 'sudo update-grub' aus.

6. Modifizieren Sie die Boot-Zeile des Kernels für Grub, um zusätzliche Kernel-Parameter für Azure einzubinden. Öffnen Sie dafür "/etc/default/grub" in einem Texteditor. Suchen Sie nach der Variablen `GRUB_CMDLINE_LINUX_DEFAULT` (oder fügen Sie diese gegebenenfalls hinzu), und bearbeiten Sie sie, um die folgenden Parameter einzubinden:

		GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"

	Speichern und schließen Sie diese Datei. Führen Sie danach "`sudo update-grub`" aus. Dadurch wird sichergestellt, dass alle Konsolennachrichten zum ersten seriellen Port gesendet werden. Dieser kann Azure bei der Behebung von Fehlern in technischer Hinsicht unterstützen.

8.	Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Booten hochfährt. Dies ist für gewöhnlich die Standardeinstellung.

9.	Installieren des Azure Linux Agent:

		# sudo apt-get update
		# sudo apt-get install walinuxagent

	Beachten Sie, dass durch Installieren des Pakets `walinuxagent` die Pakete `NetworkManager` und `NetworkManager-gnome` entfernt werden, wenn sie installiert sind.

10.	Führen Sie die folgenden Befehle aus, um den virtuellen Computer zurückzusetzen und ihn für die Bereitstellung in Azure vorzubereiten:

		# sudo waagent -force -deprovision
		# export HISTSIZE=0
		# logout

11. Klicken Sie im Hyper-V-Manager auf **Aktion -> Herunterfahren**. Ihre Linux-VHD kann nun in Azure hochgeladen werden.

## Nächste Schritte
Sie können jetzt mit Ihrer Ubuntu-Linux-VHD-Datei neue virtuelle Azure-Computer in Azure erstellen. Wenn Sie die VHD-Datei zum ersten Mal in Azure hochladen, führen Sie die Schritte 2 und 3 in [Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält](virtual-machines-linux-classic-create-upload-vhd.md) aus.

## Referenzen ##

Ubuntu HardWare Enablement-Kernel (HWE)

- [http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

<!---HONumber=AcomDC_0518_2016-->