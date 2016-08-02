#R Cheatsheet

```r
nrow(alle)
colnames(alle)
str(alle)
summary(alle)
head(alle)
summary(as.numeric(alle$price))
```


```r
df = data.frame(summary(alle$start))
rownames(df)
```

```r
merged = merge(df,cities)
summary(merge(df,cities))
```


```r
plot(summary(cities[,2]))
```


```r
par(mar=c(5,10,2,2))
plot(summary(cities[,2]),1:21,yaxt="n",ylab="")
axis(2, at=1:length(unique(cities[,2])),labels=unique(cities[,2]), las=1)



parseCsv2 = function(path){
  df = read.delim(path, header=FALSE) # Read CSV
  colnames= c("history_action_id","ts_init","init_date_key","init_date_id","ts_lastchange")
  colnames(df) = colnames
  df$start_date = as.Date(as.POSIXct(df$ts_init, origin="1970-01-01")) # Convert to  date
  return(df)
}


loadFolder = function(folder="C:/Users/nkreiling/Downloads/bdh_out/see_action_log"){
  temp = list.files(path=folder, pattern="*.csv") # Load all files
  for (i in 1:length(temp)) {
	assign(temp[i], parseCsv2(paste(folder,temp[i],sep="/")))
  }
  return (temp)
}

```







