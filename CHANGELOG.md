# Changelog
**All notable changes to MC project from 10.2.2 version will be documented in this file.**

### Documentation
Chapter **8.3.4 DPAA2 User Manual** in [LSDK User Guide](https://www.nxp.com/docs/en/user-guide/LSDKUG_Rev19.09.pdf "LSDK User Guide") 

## [10.20.2] - 2020-02-27
### Fixed
- Fix for **WRIOP** resource allocation on LX2 devices – this issue was introduced in **MC 10.20.1** and was causing performance
  degradation in various scenarios
- Fix ports discovery on **Serdes1** protocol 0x39 on LS2 devices
- Fix for `dpseci_reset`
- Allow setting **OPR** using `dpseci_set_opr API` while **DPSECI** is enabled

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.20.2?h=mc_release_10.20.2 "API")

## [10.20.1] - 2020-02-03
### Added
- **Added support for RGMII in enet_if field within DPC file**
- **CAAM: initialize RNG with prediction resistance support**

### Fixed
- **LX2: Fix PCS RS-FEC initialization for 100G**

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.20.1?h=mc_release_10.20.1 "API")

## [10.20.0] - 2019-12-20
### Added
- **Enhanced debug capabilities**
	* Added **DPDBG** object and integrated it with restool
	* A single **DPDBG** object can be created and can only be added in a root container
	* `ls-debug` helper script was added to allow users to easily enable/disable the **MC** log and UART console, change the logging level, 
	   the uart_id and commands timestamps, dump the **DPNI** information as well as the allocation of PEB and DDR memory within **MC**
	* `ls-debug` script automatically creates a **DPDBG** object if one is not already created (either from **DPL** or using `restool`)
- **LX2**
	* Support for **FC-FEC**
	* Export **FEC** mode in `dpmac_get_attributes`
	* Increased number of **DPIO** objects that can be created to **65**
- **DPNI**
	* Send `DPNI_IRQ_EVENT_ENDPOINT_CHANGED` on all [dis]connect commands
- **DPMAC**
	* Added missing flags in the **APIs** header files: `DPMAC_IRQ_EVENT_LINK_UP_REQ`, `DPMAC_IRQ_EVENT_LINK_DOWN_REQ`
- **DPRC**
	* Added `dprc_set_locked` **API**
		* This **API** can only be called for a child container; a container cannot call the **API** on itself nor can it call it on its grand children
		* Lock signifies that the child container as well as the underneath containers hierarchy are not allowed to: 
		**create/destroy** objects, **assign/unassign** objects or **lock/unlock** child containers
	* `DPRC_OBJ_CREATE_ALLOWED` create option was removed; containers are now allowed to create objects by default and 
		to restrict object creation one should call the new **API** `dprc_set_locked`
- **DPDMUX**
	* Added default interface field to the **DPDMUX** create command the purpose being to allow traffic to pass 
		through the **DMUX** using a specified interface ID without the need to call `DPDMUX_IF_SET_DEFAULT`
- **DPRTC**
	* Added new **APIs**
		* `dprtc_get_ext_trigger_timestamp`: returns the timestamp and verifies if there is a pending timestamp in the **FIFO**
		* `dprtc_set_fiper_loopback`: loops the **FIPER** pulse back into the external trigger input
	* Updated `dprtc_set_irq_mask`: added support to enable **ETS1/ETS2** and **PPS2** interrupts

### Fixed
- **DPNI**
	* Updated `dpni_set_taildrop` **API** description
	* Fix flow control clean-up upon **DPNI-DPNI** disconnect
- **DPMAC**
	* Fixed an issue which was causing the `*_LINK_DOWN_REQ` interrupt to not be sent
- **DPRC**
	* Destroy command now fails if one of the objects in the container is **plugged**
- **DPMCP**
	* Fixed a bug which was causing the **DPMCP** IDs to not be freed up when the container hosting that **DPMCP** object was destroyed
- Fixed an issue which was causing internal resources to get leaked when connect fails; the issue was affecting **DPNI**, **DPSW** and **DPDMUX** objects

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.20.0?h=mc_release_10.20.0 "API")
	
## [10.19.0] - 2019-11-08
### Added
- **LX2**
	* Support for **SFI** (on **XFI** ports) – configurable via **DPC**
	* Configurable equalization settings – via **DPC**
	* Personality support (SVRs) for **LX2160** rev2
- **DPNI**
	* Option to create **OPR** per frame queue – allows creation of more than 2 **DPNIs** with `DPNI_OPT_HAS_OPR` option set
	* Increased maximum extract params to 20
	* Allow the use of different Flow Steering key composition for each Traffic Class
	* Updated the following **APIs** in **DPAA2UM**: `DPNI_CREATE`, `DPNI_SET_OPR`, `DPNI_GET_OPR`
- **DPSW**
	* Added `DPSW_CTRL_IF_SET_QUEUE` command for setting control interface queue destinations to **DPIO**
- **DPSECI**
	* Updated the following **APIs** in **DPAA2UM**: `DPSECI_SET_RX_QUEUE`, `DPSECI_SET_OPR`, `DPSECI_GET_OPR`
- **DPDMUX**
	* Updated the following **API** in **DPAA2UM**: `DPDMUX_ADD_CUSTOM_CLS_ENTRY`
- Set **AMQ** parameters for **QBman** to allow it to work with **IOMMU** enabled
- Added timestamping of every command in **MC** (log level INFO)
- Dump **PEB** memory utilization in **MC** log on DEBUG level
- Increased number of object connections (more objects can now be connected)

### Fixed
- **LX2**
	* Fixed an issue where wrong equalization settings were applied to **USXGMII** interfaces in certain conditions
- **DPNI**
	* Fixed an issue where **PFC** frames were sent for all Traffic Classes and not just for the ones that were enabled
- **DPSW**
	* Fix for Pause Frames filtering
	* Fixed an issue causing the **FLC** of incoming frames from control interface of the **DPSW** to be all zeroes
- **DPSECI**
	* Fixed an issue causing `DPSECI_SET_RX_QUEUE` **API** to fail
- **DPDMUX**
	* Fixed an issue due to which multiple classification entries could not be added when **DPDMUX** is created with `DPDMUX_OPT_CLS_MASK_SUPPORT` option
- Disabled **MAC1** on **LS1044A** and **LS1048A** personalities
	
#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.19.0?h=mc_release_10.19.0 "API")

## [10.18.0] - 2019-08-30
### Added
- **LX2**
	* Enabled **RS-FEC** by default on 100G MACs
	* Added support for all **LX2** personalities (**LX2160, LX2080, LX2120**)
- **DPNI** Classification enhancements
	* New options added to VLAN and MAC filters to configure Frame Queue and Traffic Class
	* Size of the keys in the QoS Table extended to up to 56bytes
- Added new **DPNI** command `dpni_get_link_cfg` to allow interrogation of the link configuration
- Disabled ASYM PAUSE for **DPNI-DPNI** connections
- Export the number of frames pending in the TX queues via `dpni_get_statistics` **API**

### Fixed
- For all **LX2** personalities - resolved wrong calculation of Serdes lanes when using Serdes protocol 14
- Disabled **DPAIOP** object creation on platforms which do not have an **AIOP** block
- Fix for **1588** timestamping configuration: in some cases the config was not applied in hardware therefore in common scenarios such as when using Linux kernel and **DPNIs** are connected to **DPMACs** via **DPL** file the incoming frames had invalid timestamping information in frame annotation 
- Fixed an issue which was causing incoming EOFHE error frames to be sent back to TX if the **DPNI** is configured to receive frames with this type of error 
 
#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.18.0?h=mc_release_10.18.0 "API")

## [10.17.0] - 2019-07-31
### Added
- **LX2:** Support for stashing source and stashing set using core id 
- **DPNI:** support for QoS table to accept a specific queue as destination 
- **DPSW:** Add interfaces in replicator at connect rather than initialization time 

### Fixed
- Fixed an issue which was affecting the ethernet ports when using **UEFI** bootloader: **DPNIs** could not be connected to some **DPMACs** 
- Default closing of opened objects during container reset 
- Fixed an issue which was causing MC to hang when `num_tcs` **DPNI** parameter exceeded the maximum allowed per platform 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.17.0?h=mc_release_10.18.0 "API")


## [10.16.2] - 2019-06-22
### Fixed
- This release fixes a bug which in some cases causes frames to be filtered out upon exit from the L2Switch. 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.16.2?h=mc_release_10.18.0 "API")


## [10.16.1] - 2019-06-13 
### Fixed
- Trigger SerDes receiver lane reset in specific conditions 
- **USXGMII** link timer updates 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.16.1?h=mc_release_10.18.0 "API")


## [10.16.0] - 2019-05-31
### Added
- **DPNI:** per queue buffer pool configuration (LX2 feature) 
- **DPRTC:** exported register base address and 1588 block endianess 

### Fixed
- Workaround for **TKT0508412** (mEMAC errata): filter pause frames within **DPSW** and **DPDMUX** 
- Fix for Soft Parser multiprotocol support 
- Fixed an issue which was not allowing the creation of more objects of a certain type 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.16.0?h=mc_release_10.18.0 "API")


## [10.15.1] - 2019-05-7 
### Added
- Support for **QDMA** Route by Port (new **DPRC** option to allow setting of Privileged Level on **DPDMAI** TX queues) 
- Support for **LX2160E** personality 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.15.1?h=mc_release_10.18.0 "API")


## [10.15.0] - 2019-04-19  
### Added
- Support in **DPNI** to send an MSI when connected/disconnected to/from a **DPMAC**. 
- Updates of **WRIOP** resources on **LX2** for 40G ports and above 
- New **DPMAC** API which allows query of the MAC address 
- A resource calculator spreadsheet was developed to help internal teams better design DPAA2 based systems and understand HW resources utilization. 
  The spreadsheet will be shared with internal teams separate from this release 
- Added support for **LS2080** personality 

### Fixed
- Fix for the frames getting stuck in **WRIOP** port after cable is unplugged; the fix covers several use cases including **DPDK** and **AIOP**
- Implemented a workaround for an errata affecting mEMAC ports on LX2 rev1 platform 
- Fix in Qman driver: resynchronize management command valid bit after timeout 
- `pcs_autoneg` DPC option can now be used for PHY-less **SGMII**
- Upper layer drivers can query the MAC address from **DPMAC** upon connection state change

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.15.0?h=mc_release_10.18.0 "API")


## [10.14.3] - 2019-03-01
### Changed
 DPAA2 User Manual was also updated, the complete list of changes can be found in the Revision History section of the document. 
 
### Fixed 
- **DPNI** options get corrupted when flow control is enabled and disabled 
- Pause frames are generated endlessly on 25G port (MAC5) of **LX2160** after interface is brought down and back up again in Linux 
- Traffic not passing through the MAC4 port (**XFI/USXGMII**) on **LX2160** 
- AIOP not loading in some scenarios on **LS1088** 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.14.3?h=mc_release_10.18.0 "API")


## [10.14.2] - 2019-02-15 
### Changed
- Decrease minimum DDR memory requirements on LS platforms to 128M 

### Fixed
- Fix propagation of the qbman errors to caller functions 
- Fix configuration of hash distribution 
- Fix dpni disable under traffic 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.14.2?h=mc_release_10.18.0 "API")


## [10.14.1] - 2019-01-28  
### Added
- In this release two new options were added in **DPC**: 
	* `pfdr_peb_size_kb` – allows users to configure the size (in KB) of PFDR entries in PEB memory; PFDRs in DDR are not affected 
	* `fec_mode` – this is a per port option and allows users to control FEC (Forward Error Correction) mode; this option is only available for 25G ports on **LX2160** platform and only RS-FEC mode is currently supported 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.14.1?h=mc_release_10.18.0 "API")


## [10.14.0] - 2019-01-12
### Added
- This release adds two new features (support for enqueue to VLFQ and Congestion Group support per Frame Queue) which boost **DPDK** performance in some scenarios on **LX2** platform. 
- Support added to enable per **DPNI** Flow Control on up to 8 **DPNI-DPNI** connections. 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.17.0?h=mc_release_10.14.0 "API")


## [10.13.0] - 2018-12-3
### Changed
In this release the TC Flow Control Configuration mode was changed to allow per **DPNI** backpressure 
on recycle port-based connections. Currently this support is limited to a maximum of 4 **DPNI-DPNI** connections. 

### Added
Two new commands were added to **DPAIOP** object to allow users to enable/disable the reset function of the object. 
These commands are needed in some scenarios when container reset is performed and **AIOP** should continue to run therefore `dpaiop_reset` should not perform the reset operation. 

### Fixed
A fix is also available for a context fault error seen in Linux kernel which was caused by **MC** writing to **AIOP** DDR memory. 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.13.0?h=mc_release_10.18.0 "API")


## [10.12.0] - 2018-11-5 
### Added
- This release adds support for Soft Parser on **LS1088** and **LS2088** platforms. 
	* A new object **DPSPARSER** has been added and users can load a Soft Parser Blob (binary) using the appropriate API. 
	* The legacy Soft Parser support implemented per **DPNI** is still supported but it will not be maintained going forward, users should use the newly added support. 
	  The new and legacy Soft Parser variants should not be used together. 
- User Manual updates:  
	* Added new sub-chapter: *3.5 Minimum memory requirements*
	* Updated description of total_bman_buffers field in sub-chapter *24.4.1 Child node: qbman* 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.12.0?h=mc_release_10.18.0 "API")


## [10.11.2] - 2018-10-9 
### Fixed
- Full amplitude and de-emphasis ratio of 1 for 40G interfaces.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.11.2?h=mc_release_10.18.0 "API")


## [10.11.1] - 2018-09-26 
### Added 
- Amplitude increase on 40G port on LX2 resulting in improved stability of the port
- RS-FEC support by default on 25G interfaces on LX2 – fixes errors otherwise seen during traffic
(RS-FEC cannot be disabled currently)
- Pause frame support for managed PHYs.

### Fixed
- Remapping of ports to CEETM instances and LNI groups on LX2 resulting in increased
performance in some scenarios where multiple 25G ports are used.
- Added non-zero default burst size when configuring TX shaping per DPNI

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.11.1?h=mc_release_10.18.0 "API")


## [10.11.0] - 2018-09-03 
### Added
The advertising support allows setting of multiple speeds for **USXGMII** mode: 10M/100M/1G/2.5G/10G.

### Fixed
This release addsshaping fixes on LX2 and support for auto negotiated advertising information for **DPMAC**
and **DPNI**.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.11.0?h=mc_release_10.18.0 "API")


## [10.10.0] - 2018-08-14  
### Changed
This release enables dpni objects to usecustom valuesfor TPID field in Ethernet 802.1q frames.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.10.0?h=mc_release_10.18.0 "API")


## [10.9.3] - 2018-08-06 
### Added
This release adds support for **USXGMII** on **LX2** platforms. Current supported speed is 10G.
Serdes lane reset on **LX2** is now performed if the following two conditions are met: CDR not locked or PCS
reports link down.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.9.3?h=mc_release_10.18.0 "API")


## [10.9.2] - 2018-07-13
### Changed
Update the initialization of available **WRIOP** resources when link speed is 100Gb on **LX2160** platform.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.9.2?h=mc_release_10.18.0 "API")


## [10.9.1] - 2018-07-05 
### Changed
- The CDR (Clock and Data Recovery) in Serdes block is occasionally not getting locked and to force the lock
a reset of the Serdes lanes is required.
The reset is performed during MC boot time as well as every second only for the lanes associated with the
**DPMACs** declared in **DPL** and defined as **TYPE_FIXED** in **DPC**.
The reset decision is taken upon CDR lock: if the lock is taken then no reset is performed, if not taken then
a reset is performed.
Along with Serdes lane reset a protocol reset is also performed for 25G, 40G, 50G and 100G ports on
**LX2160** platform.
- Lane reset is performed for **LS1088** and **LX2160** platforms.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.9.1?h=mc_release_10.18.0 "API")


## [10.9.0] - 2018-06-27  
### Added
- Support in **DPNI** create for separate number of RX and TX TCs

### Fixed
- **LX2:**
	* Fix for MC UART
	* Fixed PSS registers to reflect proper rate for 50G and 100G ports
	* When Serdes enables ports with the same ID as the **RGMII** ports, the Serdes ports
prevail over **RGMII** ports (on LX2, some Serdes combinations enable SGMIIs 17 and
18 which overlap RGMIIs)
- **DPSW**: Set per port default tail-drop values – fixes an occasional ~12% line rate drop due to
clock skew between LS boards and the testing equipment
- Fixed an issue which was causing TX confirmations to be delivered to wrong cores on LX2
and 40G MAC.2 was impacting the **RGMII** ports

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.9.0?h=mc_release_10.18.0 "API")


## [10.8.2] - 2018-06-18  
### Added
This release adds support for multiple Serdes protocols for LX2160A and properly sets the maximum
recommended values of **WRIOP** Ports resources.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.8.2?h=mc_release_10.18.0 "API")


## [10.8.1] - 2018-05-31  
### Changed
This release adds support for **LX2160A** Si as well as fixes and enhancements to **DPNI** object’s statistics.
Highlights:
- **LX2160A**
	* Support for **RGMII**, **XFI**, 25G, 40G
	* Serdes protocols supported: Serdes1 19
- New dpni statistics: page_4 contains number of dropped frames/bytes caused by congestion
points; this change does not modify the ABI.
- New dpni statistics: page_5 contains number of policer colored frames; this change does not
modify the ABI.
- New option `DPNI_ERROR_DISC` that will send all discarded frames into error queue; this
change does not modify the ABI. 

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.8.1?h=mc_release_10.18.0 "API")


## [10.8.0] - 2018-05-15 
### Added
This release brings new features and documentation updates.
Highlights:
- **DPNI QOS**: new parameter added to `DPNI_SET_QOS_TABLE` command
	* keep_entries – will not clear existing rules when key is changed
- Increased number of queues for **DPDMAI**
	* Maximum number of queues equals with number of platforms cores 
	(16 for **LX2160**, 8 for **LS2088**, **LS1088**, **LS2080/LS2085**)
- New options added to **DPBP** notifications:
	* `DPBP_NOTIF_OPT_WRIOP` and `DPBP_NOTIF_OPT_AIOP`

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.8.0?h=mc_release_10.18.0 "API")


## [10.7.0] - 2018-04-05 
### Changed
This release brings new features as well as bug fixes and documentation updates.
Highlights:
- RFS/RSS split key
	* New APIs added to **DPNI** object 
	* Legacy API still available – will be removed in upcoming major MC release
- Link Aggregation support for **DPSW**
	* multiple groups with multiple ports
	* traffic forwarding between LAG groups (validated with 2 groups)
	* performance tests using 64B, 128B and 1500B frames
- Support for new **qDMA** block on LX2
- **DPSECI**: 16 queues support for LX2
- **CAAM**: Era 10 support
- **DPNI**: Error encounters during FS entry addition if `key_size` if different with previous FS entry
- S2 issues:
	* **DPSW** – unicast frames forwarded to wrong port
	* **BE Mode**: Invalid Token detected in MC for any MC API call after Object opened

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.7.0?h=mc_release_10.18.0 "API")


## [10.6.0] - 2018-01-28 
### Added
This release adds support for Link Aggregation feature for L2Switch.
LAG configuration is available via API (`dpsw_lag_set`, `dpsw_lag_get_cfg`) and DPL.
Validated LAG features and known limitations:
- Single LAG group tested; up to 8 groups will be supported, validation is ongoing
- Group of 2 ports and 4 ports validated; up to 8 ports in a single group will be supported,
validation is ongoing
- Unicast frames transmitted and received from/into the LAG group; multicast support is ongoing
- Performance validation is ongoing
- The ports that can be added in a LAG group must be in the same Qman CEETM instance
	* CEETM instance 1: {1, 2, 3, 4, 9, 10, 11, 12}
	* CEETM instance 2: {5, 6, 7, 8, 13, 14, 15, 16}
	* Dynamic reallocation of ports will be implemented to allow users to add any port to the same LAG group
	
**API** configuration example:

	```
	dpsw_lag_cfg.group_id = 1;
	dpsw_lag_cfg.num_ifs = 2;
	dpsw_lag_cfg.if_id[0] = 0; // DPSW interface 0
	dpsw_lag_cfg.if_id[1] = 1; // DPSW interface 1
	err = dpsw_lag_set(&dpsw->io, 0, dpsw->token, &dpsw_lag_cfg);
	```
**DPL** configuration example:

	```
	dpsw@1{
			….
			lag1 = <0 1>; // this will create LAG group 1 with DPSW interfaces 0 and 1
						  //added to the group
		  };
	```
### Fixed
- Fixed a bug that was limiting bandwidth through **DPDMUX** to 1G
- LX2 emulation (PXP/CFP): removed polling of unimplemented bit on cEMAC ports during RX
  graceful stop procedure – this was causing the **WRIOP** ports to remain disabled
  
#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.6.0?h=mc_release_10.18.0 "API")


## [10.5.0] - 2017-12-31 
### Changed
This release brings LX2 emulation support, new features and bug fixes.
Highlights:
- Emulation:
	* **PXP**
		* MC boot to prompt on model F4
		* **DPL** process – **DPMAC, DPNI, DPBP, DPMCP, DPIO, DPRC** objects
		* Modules initializations disabled: **SEC, QDMA, RTC, DCE**
	* **CFP**
		* MC boot to prompt on model 3c6
		* **DPL** process – **DPMAC, DPMCP, DPRC** objects
		* Modules initializations disabled: **WRIOP, SEC, RTC, DCE**
		* **WRIOP** initialization is commented out due to model issues: TKT361591 - CTLU
		commands issued from MC are failing
- Support for **QMan** memory backed portal mode
- **DPNI**:
	* New DPNI option `DPNI_OPT_SINGLE_SENDER` to allow users to set number of senders to 1
	* SW opaque field added to `dpni_get/set_buffer_layout` APIs
- Documentation:
	* Added 1000Base-X and PHY-less use cases
	* DPDMUX: added `dpdmux_if_get/set_default` APIs, custom key commands
	* DPNI: added SW opaque field added to `dpni_get/set_buffer_layout` APIs
	* DPRC: added base_address field to `dprc_get_obj_region` API

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.5.0?h=mc_release_10.18.0 "API")


## [10.4.0] - 2017-11-01 
### Added
- **LX2**:
	* QBMAN 5.0 – Support for portal migration
	* Support for 16 transmit and receive queues and enablement for 16TCs
	* **USXGMII** support (port type selection via the **DPC**)
	* Serdes registers mapping update
- **DPCI**: Support for order preservation and multiple priorities
- Support for non-E parts
- **DPDMUX**: New API to set/get the default interface

### Fixed
- Fix for a critical issue where AIOP was not getting loaded on kernel 4.9

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.4.0?h=mc_release_10.18.0 "API")


## [10.3.4] - 2017-09-29
### Fixed
This release fixes several bugs introduced in MC 10.3.3 affecting the functionality of network interfaces
configured in managed PHY mode (DPC option `MAC_LINK_TYPE_PHY`) in Linux.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.3.4?h=mc_release_10.18.0 "API")


## [10.3.3] - 2017-09-20
### Changed
This release allows users to select 1000BaseX or **SGMII** mode via the **DPC**.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.3.3?h=mc_release_10.18.0 "API")


## [10.3.2] - 2017-08-23 
### Fixed
This release fixes a memory corruption issue occurring when calling `dpni_set_tx_priorities()` API.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.3.2?h=mc_release_10.18.0 "API")


## [10.3.1] - 2017-08-05
### Added
- Proper **WRIOP** resource allocation for High Speed and Recycle ports on LX2
- Setting of **DPMACs** based on information gathered from Serdes/PSSR register
- CLI build target for **LS1088**, **LS2080** and **LA1575**

### Fixed
- Substantially improved the time needed to create **DPNI** objects
- Fixed a serious defect which was making MC unresponsive affecting AIOP loading
- Fixed cEmac statistics counters (LX2)
- Fixed a bug where MC was hanging during **DPL** parsing
- Fixed MC boot on **LS2080** platform

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.3.1?h=mc_release_10.18.0 "API")


## [10.3.0] - 2017-06-30
### Added
- Support for Priority Flow Control (PFC) triggered by congestion notification for **DPNI**
- QoS support for **AIOP**
- QoS - CEETM configuration 
	* Add support for two WBFS groups per CEETM channel
- Support for QBMan 5.0 (available on LX2160); shaping at class queue level
- Soft Parser loading support on **WRIOP** Parser
- Support for **AIOP** style FLC
- Added CLI support as separate build target (for debugging purpose only; not available in release
  binary) – available on **LS2088** (support for the rest of the platforms is work in progress)

### Fixed  
- **LX2160**: fixed HighSpeed MAC statistics, **RGMII** ports discovery (ports #17 and #18), recycle ports
  IDs updated to #19 and #20

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.3.0?h=mc_release_10.18.0 "API")


## [10.2.2] - 2017-05-26 
### Fixed
- This is a bug fix release addressing severity 2 and 3 issues.
- High Speed MACs have also been enabled on **LX2160A** platform.

#### [API](https://source.codeaurora.org/external/qoriq/qoriq-components/mc-utils/tree/api/mc_release_10.2.2?h=mc_release_10.18.0 "API")
