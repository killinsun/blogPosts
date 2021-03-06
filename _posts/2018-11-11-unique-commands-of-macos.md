---
ID: 231
post_title: macOS 固有CLI コマンド
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2018/11/unique-commands-of-macos/
published: true
post_date: 2018-11-11 18:33:21
---
意外とLinux系のコマンドは触ることが多いが、普段よく使うmacOS固有のコマンドって「打ってみて」と言われてすぐに手が動かせないので備忘録
<h2>全般</h2>
<h3>system_profiler (システム構成確認)</h3>
要は　→このMacについて→システムレポート　の内容を出力
<pre class="lang:default decode:true">system_profiler</pre>
ただしこのまま打つと情報量が多いので、lessに流すとかして使うか、DataTypeというものを指定してやる。
<pre class="lang:default decode:true" title="DataTypeの一覧">Available Datatypes:
SPParallelATADataType
SPUniversalAccessDataType
SPSecureElementDataType
SPApplicationsDataType
SPAudioDataType
SPBluetoothDataType
SPCameraDataType
SPCardReaderDataType
SPComponentDataType
SPiBridgeDataType
SPDeveloperToolsDataType
SPDiagnosticsDataType
SPDisabledSoftwareDataType
SPDiscBurningDataType
SPEthernetDataType
SPExtensionsDataType
SPFibreChannelDataType
SPFireWireDataType
SPFirewallDataType
SPFontsDataType
SPFrameworksDataType
SPDisplaysDataType
SPHardwareDataType
SPHardwareRAIDDataType
SPInstallHistoryDataType
SPLegacySoftwareDataType
SPNetworkLocationDataType
SPLogsDataType
SPManagedClientDataType
SPMemoryDataType
SPNVMeDataType
SPNetworkDataType
SPPCIDataType
SPParallelSCSIDataType
SPPowerDataType
SPPrefPaneDataType
SPPrintersSoftwareDataType
SPPrintersDataType
SPConfigurationProfileDataType
SPRawCameraDataType
SPSASDataType
SPSerialATADataType
SPSPIDataType
SPSmartCardsDataType
SPSoftwareDataType
SPStartupItemDataType
SPStorageDataType
SPSyncServicesDataType
SPThunderboltDataType
SPUSBDataType
SPNetworkVolumeDataType
SPWWANDataType
SPAirPortDataType</pre>
&nbsp;

個人的によく使うのはこれ
<h4>system_profiler SPUSBDataType (USBポートの情報を表示)</h4>
<pre class="lang:default decode:true">$ system_profiler SPUSBDataType
USB:

    USB 3.0 Bus:

      Host Controller Driver: AppleUSBXHCISPTLP
      PCI Device ID: 0x9d2f
      PCI Revision ID: 0x0021
      PCI Vendor ID: 0x8086

        USB2.0 Hub:

          Product ID: 0x2813
          Vendor ID: 0x2109  (VIA Labs, Inc.)
          Version: 90.11
          Speed: Up to 480 Mb/sec
          Manufacturer: VIA Labs, Inc.
          Location ID: 0x14300000 / 7
          Current Available (mA): 500
          Current Required (mA): 0
          Extra Operating Current (mA): 0

            UX1:

              Product ID: 0x4150
              Vendor ID: 0x0e41
              Version: 0.01
              Speed: Up to 12 Mb/sec
              Manufacturer: Line 6
              Location ID: 0x14340000 / 10
              Current Available (mA): 500
              Current Required (mA): 498
              Extra Operating Current (mA): 0

            USB 2.0 BILLBOARD             :

              Product ID: 0x0100
              Vendor ID: 0x2109  (VIA Labs, Inc.)
              Version: 6.00
              Serial Number: 0000000000000001
              Speed: Up to 480 Mb/sec
              Manufacturer: VIA Technologies Inc.
              Location ID: 0x14310000 / 9
              Current Available (mA): 500
              Current Required (mA): 100
              Extra Operating Current (mA): 0

    USB 3.1 Bus:

      Host Controller Driver: AppleUSBXHCIAR
      PCI Device ID: 0x15d4
      PCI Revision ID: 0x0002
      PCI Vendor ID: 0x8086
      Bus Number: 0x00

        USB3.0 Hub:

          Product ID: 0x0813
          Vendor ID: 0x2109  (VIA Labs, Inc.)
          Version: 90.11
          Speed: Up to 5 Gb/sec
          Manufacturer: VIA Labs, Inc.
          Location ID: 0x00200000 / 1
          Current Available (mA): 900
          Current Required (mA): 0
          Extra Operating Current (mA): 0

            USB3.0 Card Reader:

              Product ID: 0x0749
              Vendor ID: 0x05e3  (Genesys Logic, Inc.)
              Version: 15.35
              Serial Number: 000000001536
              Speed: Up to 5 Gb/sec
              Manufacturer: Generic
              Location ID: 0x00220000 / 2
              Current Available (mA): 900
              Current Required (mA): 896
              Extra Operating Current (mA): 0</pre>
<h2>ネットワーク関連</h2>
<h3>networksetup -listallhardwareports (ハードウェア情報表示)</h3>
<pre class="lang:default decode:true">$ networksetup -listallhardwareports

Hardware Port: Wi-Fi
Device: en0
Ethernet Address: XX:XX:XX:XX:XX:XX

Hardware Port: Bluetooth PAN
Device: en3
Ethernet Address: XX:XX:XX:XX:XX:XX

Hardware Port: Thunderbolt 1
Device: en1
Ethernet Address:XX:XX:XX:XX:XX:XX

Hardware Port: Thunderbolt 2
Device: en2
Ethernet Address: XX:XX:XX:XX:XX:XX

Hardware Port: Thunderbolt Bridge
Device: bridge0
Ethernet Address: XX:XX:XX:XX:XX:XX

VLAN Configurations
===================</pre>
<h3>airport -s (無線アクセスポイントをスキャンして一覧表示)</h3>
かなり階層が深い所にあるコマンド。
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/

初期セットアップ時にエイリアス張ったほうがいい。
<pre class="lang:default decode:true">$ /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s
                            SSID BSSID             RSSI CHANNEL HT CC SECURITY (auth/unicast/group)
                   HUMAX-2B218-A XX:XX:XX:XX:XX:XX -66  136,-1  Y  JP WPA(PSK/AES,TKIP/TKIP) WPA2(PSK/AES,TKIP/TKIP)
      NETGEAR85-5GHz-guestAccess XX:XX:XX:XX:XX:XX -39  128     Y  JP WPA2(PSK/AES/AES)
                     HUMAX-2B218 XX:XX:XX:XX:XX:XX -56  11      Y  JP WPA(PSK/AES,TKIP/TKIP) WPA2(PSK/AES,TKIP/TKIP)
                 elecom5g-76fc61 XX:XX:XX:XX:XX:XX -68  48      Y  US WPA2(PSK/AES/AES)
                  aterm-a2ba1f-a XX:XX:XX:XX:XX:XX -81  48      Y  JP WPA(PSK/AES/AES) WPA2(PSK/AES/AES)
                  NETGEAR85-5GHz XX:XX:XX:XX:XX:XX -42  44      Y  JP WPA2(PSK/AES/AES)</pre>
&nbsp;
<h2>ディスク関連</h2>
このあたりはよく紹介されていて使う頻度が高いと思う
<h3>diskutil list (ディスク一覧を表示)</h3>
fdisk -l　的な。
<pre class="lang:default decode:true">$ diskutil list
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         500.3 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:                 Apple_APFS Container disk1         500.0 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +500.0 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD            259.6 GB   disk1s1
   2:                APFS Volume Preboot                 46.1 MB    disk1s2
   3:                APFS Volume Recovery                512.8 MB   disk1s3
   4:                APFS Volume VM                      2.1 GB     disk1s4

/dev/disk2 (disk image):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        +4.1 TB     disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:                  Apple_HFS Time Machineバッ...     4.1 TB     disk2s2</pre>
&nbsp;
<h3>sudo diskutil mount （ディスクのマウント）</h3>
<pre class="lang:default decode:true">sudo diskutil mount -mountPoint &lt;&lt;path_to_mount_point&gt;&gt; &lt;&lt;device&gt;&gt;　　
</pre>
デバイス名は/dev/disk2s1とかそういった単位

&nbsp;
<h3>sudo diskutil unmount (ディスクのアンマウント)</h3>
<pre class="lang:default decode:true ">sudo diskutil unmount &lt;&lt;device&gt;&gt;</pre>
&nbsp;

&nbsp;

あとは思いついたら書く