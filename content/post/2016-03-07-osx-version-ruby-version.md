+++
title = "OSXのバージョンとjsxとrubyバージョン"
date = "2016-03-07T00:00:00+09:00"
tags = ["osx", "extendscript", "ruby"]
+++

Extendscriptで do shell script経由で内蔵Ruby使う時、

- 10.8 ruby1.8
- 10.9 ruby1.8/2.0 デフォルトは2.0
- 10.10 ruby2.0
- 10.11 ruby2.0

となっているので、

```js
var os_major;
var os_ver = $.os.match(/Macintosh OS\s(\d+\.\d+\.\d+)/)[1];
if (os_ver !== null) {
  os_major = os_ver.split(".")[1];
}
```

でOSXのバージョン分岐するより

ruby1.8/ruby2.0の有無を `File(/path/to/ruby).exists` で分岐したほうがよさげだった。

```js
var rb18_path = "/System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/ruby";
var rb20_path = "/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby";

var shellscript;
if (File(rb20_path).exists) {
  shellscript = rb20_path + " -rbase64 -e 'File.open(\\\"" + path + "\\\",\\\"wb\\\"){|f| f.print Base64::decode64(File.read(\\\"" + base64_file + "\\\"))}'"
}
else if (File(rb18_path).exists) {
  shellscript = rb18_path + " -rbase64 -e 'File.open(\\\"" + path + "\\\",\\\"wb\\\"){|f| f.print Base64::decode64(File.read(\\\"" + base64_file + "\\\"))}'"
}
else {}
```


ベンチ


```rb
require 'benchmark'
require 'base64'

# bench_base64.rb

puts RUBY_DESCRIPTION
BenchTimes = 500

base64_str = ""
Benchmark.bm(20) do |bm|
  
  bm.report('encoding base64') do
    BenchTimes.times{ base64_str = Base64::encode64(File.read(File.expand_path "~/Desktop/x.psd")) }
  end

  bm.report('decoding base64') do
    BenchTimes.times{ Base64::decode64(base64_str) }
  end
end
```

```
% ruby bench_base64.rb
ruby 2.3.0p0 (2015-12-25 revision 53290) [x86_64-darwin13]
                           user     system      total        real
encoding base64        5.120000   2.480000   7.600000 (  7.597840)
decoding base64        5.990000   0.550000   6.540000 (  6.543969)

% /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby bench_base64.rb
ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin13]
                           user     system      total        real
encoding base64        5.900000   2.120000   8.020000 (  8.013749)
decoding base64        6.390000   0.360000   6.750000 (  6.744955)

% /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/ruby bench_base64.rb
ruby 1.8.7 (2012-02-08 patchlevel 358) [universal-darwin13.0]
                          user     system      total        real
encoding base64       5.120000   2.570000   7.690000 (  7.690962)
decoding base64       5.640000   0.540000   6.180000 (  6.184804)
```


