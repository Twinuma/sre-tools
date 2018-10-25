---
title: "Linux Command"
date: 2018-10-25T16:19:00+09:00
draft: false
---
## CPU使用率TOP10を表示する
`ps -e aux | sort -r -k 3 | head -n 10`

## メモリ占有率TOP10を表示する
`ps -e aux | sort -r -k 4 | head -n 10`

## 項目名がウザい場合は以下の文字列を付与する
`ps --no-header`

## CPU使用率TOP20を項目名無しでかつ、整形したデータを表示する
`ps --no-header -e aux | sort -r -k 3 | head -n 20 | awk '{print $1,$3,$11;};'`

## メモリ占有率TOP20を項目名無しでかつ、整形したデータを表示する
`ps --no-header -e aux | sort -r -k 4 | head -n 20 | awk '{print $1,$4,$11;};'`

## 現在のメモリ状況を確認する
```
$ grep Mem /proc/meminfo
$ grep Swap /proc/meminfo
```

## 容量をくってるベスト１００を表示
`du -ma | sort -rn | head -100`

## ディレクトリのみを検索する場合
`du -m | sort -rn | head -100`

## ファイヤウォールの設定を確認する
`iptables -nv -L`

## ネットワーク接続数、接続元、接続先、実行プロセスとPID確認
`ss -lnp`

## プロセス確認
`ps aux | grep -w 1380 | grep -v grep`

## scriptによる作業ログスクリプト

```
#!/bin/bash
NOW=$(daate +%Y%m%d_%H%M%S)
exec script -q -c "ssh $1" $1.$NOW.log
```

```
chmod a+x lssh
./lssh localhost
```

## システム稼働時間、ログインユーザ数、ロードアベレージを確認
```
w
```

## 起動プロセス確認
```
ps aux
```

## ディスク使用量
```
df -h
```

## CPU使用率、メモリ使用量、CPU使用率が高いプロセス確認
```
top -b -d 1 -n 1
```

## メモリ使用量が大きいプロセス確認
```
top -b -d 1 -n 1 -a
```

## CPU使用率、ネットワーク使用量、ディスクのI/O量、ページング量
```
dstat -taf 1 10
```

## １秒毎にロードアベレージ、メモリ、CPU、ネットワークの情報を表示
```
dstat -tlamf 1
```

## CPUの状況を表示
```
top
```
S(Status)がR(実行中)やD(読み書き中)の場合は、CPUをかなり積極的に使っている

%CPUは100%が1コア分なので、4コアのサーバであれば最大400%まで上がる。


## ディスクI/Oを表示
```
iostat -tnx 1
```
デバイスごとに利用状況が表示されるので、特に右端の「%util」が大きくなっている場合はディスクIO要求に対して性能が足りていない

## ネットワークインターフェイスのeh0からきたHTTPアクセスをキャプチャする
```
tcpdump -i eh0 -n port 80
```

## プロセス確認
```
ps aufx | grep http[d] | head -3
strace -f -p 5377
lsof -p 5377
```

## OSのCPU利用率を下げる
```
iotop
iotop -P 
```

## curl tips
### 証明書無視
```
curl -I https://114.111.79.198/config/login -k
```

## dateコマンドでバックアップファイル名末尾に日付_時間を付ける
```
cp -p ./nginx.conf ./nginx.conf.`date "+%Y%m%d_%H%M%S"`
```


## 参考リンク
- [http://easyramble.com/linux-command-to-check-status.html](http://easyramble.com/linux-command-to-check-status.html)
- [http://qiita.com/k0kubun/items/8ab1dfa7c0359d8e618d](http://qiita.com/k0kubun/items/8ab1dfa7c0359d8e618d)
- [http://hosii.net/?p=403#i-5](http://hosii.net/?p=403#i-5)