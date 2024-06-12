# Ansible role - cf-ssh

Installation and configuration of SSH.

## Synopsis

This role install SSH daemon and configures it regarding to the Benchmarks available from BSI and CSI.

## Parameters

| Parameter             | Choices/Defaults     | Comments                                                                                                                                |
| --------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| ssh_allow_tcp_forward | - yes<br>- **no** <- | Let you choose if TCPForwarding is enabled or not.                                                                                      |
| ssh_max_sessions      |                      | The maximum allowed sessions. The default value is <br> "4" and should just increased when multiple people <br>has to access the server |

## See also

- [BS TR-02102-4](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Publikationen/TechnischeRichtlinien/TR02102/BSI-TR-02102-4.html) - "Recommendations for the use of SSH"
- [Openbsd man page](https://man.openbsd.org/sshd_config)
- [CIS Independant Linux Benchmark](https://www.cisecurity.org/cis-benchmarks/)
- [VSCode Remote troubleshooting](https://code.visualstudio.com/docs/remote/troubleshooting)

## Expected result

- run "nmap --script ssh2-enum-algos localhost" and check received algorithms (#TODO Create script that automatically checks ciphers)
- check the file existing permissions must be written (#TODO Create script that automatically checks existance and permissions og files)
  - check the permissions of /etc/ssh/sshd_config (600)
  - check the permissions of /etc/ssh/private keys (600)
  - check the permissions of /etc/ssh/public keys (644)
  - /etc/motd
  - /etc/issue.net
- Check the daemon configuration against shhd_config template (#TODO Create script which checks the ssh config)

## Authors

- David Koenig
