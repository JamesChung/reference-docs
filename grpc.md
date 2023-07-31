# gRPC Reference

## Handy Tips

### encode value

```
# text file value > id: 123
$ cat <text file> | protoc --encode=Account account.proto | hexdump -C
00000000  08 7b                                             |.{|
00000002
```

### decode value

```
$ cat <encoded file> | protoc --decode=Account acount.proto
id: 123
```
