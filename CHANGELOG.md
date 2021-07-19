# Changelog
**All notable changes to MC project from 10.100.22 version will be documented in this file.**


## [10.100.22]
### Fixed
- **DPNI** - Fix allocation for Tx queues on dpni-dpni connections (in some cases queues allocation was made twice and release performed only once)
- **DPNI** - Fix flow control enable on asymmetric modes (sending on flow control frames when buffer poll was depleted was always enabled)
- **DPMAC** - Remove PCS protocol reset
