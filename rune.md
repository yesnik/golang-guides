# Rune

Bytes work fine with the English alphabet characters, which each take up 1 byte.
But the Russian characters each take 2 bytes. 
Cutting off the first 1 byte of that string omits only the "half" of the first character, resulting in an unprintable character.

```go
asciiString := "Hey"
fmt.Println(asciiString, len(asciiString)) // Hey 3
utf8String := "Хай"
fmt.Println(utf8String, len(utf8String)) // Хай 6

fmt.Println(utf8.RuneCountInString(asciiString)) // 3
fmt.Println(utf8.RuneCountInString(utf8String)) // 3

asciiBytes := []byte(asciiString)
utf8Bytes := []byte(utf8String)
asciiBytesPartial := asciiBytes[1:]
utf8BytesPartial := utf8Bytes[1:]
fmt.Println(string(asciiBytesPartial)) // ey
fmt.Println(string(utf8BytesPartial)) // ?ай
```

**Important**: Anytime you want to work with just part of a string, convert it to *runes*, not *bytes*!

```go
asciiRunes := []rune(asciiString)
utf8Runes := []rune(utf8String)
asciiRunesPartial := asciiRunes[1:]
utf8RunesPartial := utf8Runes[1:]
fmt.Println(string(asciiRunesPartial)) // ey
fmt.Println(string(utf8RunesPartial)) // ай
```
