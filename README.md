# IOS 1. project
The goal of this project is to implement a shell script that will search for different transactions on the stock exchange in a CSV file with certain filters.
### Evaluation 15/15

# Usage
Make the file executable:
```console
$ chmod +x xtf
```
The CSV file must have the following columns:
```CSV
| TraderName | DateOfTransaction | Сurrency | Amount |
```
Now you can start by using:
```console
$ ./xtf --help
```
```console
$ ./xtf command -f "USER" ./files
```

- `--help` Will give a little help explanation of usage
- `command` is the command that will be used when searching and `-f` are filters that can be applied
- `"USER"` The name of the trader whose search will be carried out
- `./files` Files to be searched (also compressed) 

# Avaliable filter and command arguments
**Commands**
- `list` will write out logs with specified USER
- `list-currency` will write out logs with specified USER
- `status` will write out all currencies and their amount of specified USER
- `profit` will write out all currencies and their amount of specified USER but with profit, that is set in environment variable XTF_PROFIT
<br/>

*XTF_PROFIT is 20% by default, but procent of profit can be reset , for example, using:*
```console
$ export XTF_PROFIT=50
```
<br/>

**Filters**
- `-a DATE` will filter logs that were after specified date
- `-b DATE` will filter logs that were before specified date
- `-c CURRENCY` will filter logs that have specified currencies

# Examples
**Example CSV files**
```console
$ cat cryptoexchange.log 
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader2;2024-01-15 15:31:12;BTC;-9.8734
Trader1;2024-01-16 18:06:32;USD;-3000.0000
```
```console
$ gzip -d cryptoexchange2.log.gz
$ cat cryptoexchange2.log 
CryptoWiz;2024-01-17 08:58:09;CZK;10000.0000
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
```

**Can search in several files simultaneously, including compressed files**
```console
$ ./xtf list "Trader1" cryptoexchange.log                       
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader1;2024-01-16 18:06:32;USD;-3000.0000
```
```console
$ ./xtf list "Trader1" cryptoexchange2.log.gz 
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
```
```console
$ ./xtf list "Trader1" cryptoexchange2.log.gz cryptoexchange.log 
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader1;2024-01-16 18:06:32;USD;-3000.0000
```

**list-currency and status commands**
```console
$ ./xtf list-currency "Trader1" cryptoexchange2.log.gz cryptoexchange.log 
ETH
EUR
USD
```
```console
$ ./xtf status "Trader1" cryptoexchange2.log.gz cryptoexchange.log
ETH : 12.8954
EUR : -2000.0000
USD : -3000.0000
```

**profit command**
```console
$ ./xtf profit "Trader1" cryptoexchange2.log.gz
ETH : 15.4745
```
```console
$ export XTF_PROFIT=50 && ./xtf profit "Trader1" cryptoexchange2.log.gz
ETH : 19.3431
```

**Filters**
```console
$ ./xtf list -a "2024-01-16 16:00:00" -b "2024-01-19 17:00:00" "Trader1" cryptoexchange2.log.gz cryptoexchange.log
Trader1;2024-01-16 18:06:32;USD;-3000.0000
```
```console
$ ./xtf list -c "EUR" "Trader1" cryptoexchange2.log.gz cryptoexchange.log
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
```
