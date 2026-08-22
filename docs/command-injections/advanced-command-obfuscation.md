# Advanced Command Obfuscation

## Case Manipulation

=== "LINUX"

    ```sh
    my@attack:~$ $(tr 'A-Z' 'a-z' <<< 'WhOaMi')
    ```

=== "WINDOWS"

    ```pwsh
    PS C:\Users\my> WhOaMi
    ```

## Reversed

=== "LINUX"

    ```sh
    my@attack:~$ rev <<< 'whoami'
    my@attack:~$ $(rev <<< 'imaohw')
    ```

=== "WINDOWS"

    ```pwsh
    PS C:\Users\my> -join 'whoami'[-1..-6]
    PS C:\Users\my> IEX $(-join 'imaohw'[-1..-6])
    ```

## Encoded

=== "LINUX"

    ```sh
    my@attack:~$ echo -n 'whoami' | base64 -w 0
    my@attack:~$ bash <<< $(base64 -d <<< d2hvYW1p)
    ```

=== "WINDOWS"

    ```pwsh
    PS C:\Users\my> [Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))
    PS C:\Users\my> IEX $([System.Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))
    ```
