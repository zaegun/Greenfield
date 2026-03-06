# Operating Systems
## Windows Server
Windows server has been selected as the backbone of the server infrastructure. Most core services will Windows based, with Linux playing a subservient role in the structure.

### Core
Core should be utilized when possible to reduce server overhead. To reduce the complexity of Core integration, it is recommended to install additional services via the FOD distribution. This will enable some administrative on the OS. This should be the default version, with the desktop evaluated for specific reasons.

### Desktop
Desktop edition should be used when Core edition lacks the functionality or there are administrative reason for needed a GUI.

## Linux
### Ubuntu Pro
Not all systems are deployable via Windows Server. In these cases, Linux systems can be utilized to fill the system gaps. Ubuntu has been selected due to it's ease of use, tight Windows infrastructure integration, and extensive compliance tools.