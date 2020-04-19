# Flags

if we want nice way to get input from user we can use flag



```go
csvFilename := flag.String("csv","problems.csv","a csv file in the format of 	question,answer")

timeLimit := flag.Int("limit",30,"the time limit for the quiz in seconds")

```

what this will do is , when user run the program , he will easily add arguments 

```go
./program -csv=myc.csv -limit=10
```

## Timer

Timer - like setTImeout

Ticker - like setInterval



