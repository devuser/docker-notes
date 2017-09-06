```
def filename = (new File("foo/ISSERM.ISS999R.20160513.00.P")).name
def mapX = [:]
mapX[filename as String]= true
def bizdate = "20160513"
mapX.containsKey("ISSERM.ISS999R.${bizdate}.00.P" as String )
```


