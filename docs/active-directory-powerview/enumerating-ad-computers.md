# Enumerating AD Computers

## Most Important Properties

```pwsh
PS C:\Users\htb-student> Get-DomainComputer -Properties dnshostname,operatingsystem,lastlogontimestamp,useraccountcontrol
```

```output title="Output"
lastlogontimestamp    dnshostname                                                        useraccountcontrol operatingsystem
------------------    -----------                                                        ------------------ ---------------
11/20/2024 3:25:31 AM DC01.INLANEFREIGHT.LOCAL                 SERVER_TRUST_ACCOUNT, TRUSTED_FOR_DELEGATION Windows Server 2016 Standard
2/3/2022 12:10:42 PM  EXCHG01.INLANEFREIGHT.LOCAL                                 WORKSTATION_TRUST_ACCOUNT Windows Server 2016 Standard
2/3/2022 12:11:01 PM  SQL01.INLANEFREIGHT.LOCAL   WORKSTATION_TRUST_ACCOUNT, TRUSTED_TO_AUTH_FOR_DELEGATION Windows Server 2016 Standard
11/20/2024 3:25:34 AM WS01.INLANEFREIGHT.LOCAL                                    WORKSTATION_TRUST_ACCOUNT Windows Server 2016 Standard
7/26/2020 6:58:15 PM  DC02.INLANEFREIGHT.LOCAL                    ACCOUNTDISABLE, WORKSTATION_TRUST_ACCOUNT
```

## Exporting CSV File

```pwsh
PS C:\Users\htb-student> Get-DomainComputer -Properties dnshostname,operatingsystem,lastlogontimestamp,useraccountcontrol | Export-Csv computers.csv -NoTypeInformation
```

## Unconstrained Delegation

```pwsh
PS C:\Users\htb-student> Get-DomainComputer -Unconstrained -Properties dnshostname,useraccountcontrol
```

```output title="Output"
dnshostname                                        useraccountcontrol
-----------                                        ------------------
DC01.INLANEFREIGHT.LOCAL SERVER_TRUST_ACCOUNT, TRUSTED_FOR_DELEGATION
```

## Constrained Delegation

```pwsh
PS C:\Users\htb-student> Get-DomainComputer -TrustedToAuth | Select-Object -Property dnshostname,useraccountcontrol
```

```output title="Output"
dnshostname                                                        useraccountcontrol
-----------                                                        ------------------
EXCHG01.INLANEFREIGHT.LOCAL                                 WORKSTATION_TRUST_ACCOUNT
SQL01.INLANEFREIGHT.LOCAL   WORKSTATION_TRUST_ACCOUNT, TRUSTED_TO_AUTH_FOR_DELEGATION
```
