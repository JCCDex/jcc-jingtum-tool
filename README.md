# jcc-jingtum-tool

![npm](https://img.shields.io/npm/v/jcc-jingtum-tool.svg)
[![Build Status](https://travis-ci.com/JCCDex/jcc-jingtum-tool.svg?branch=master)](https://travis-ci.com/JCCDex/jcc-jingtum-tool)
[![npm downloads](https://img.shields.io/npm/dm/jcc-jingtum-tool.svg)](http://npm-stat.com/charts.html?package=jcc-jingtum-tool)

jcc-jingtum-tool 是一个命令行工具，可以快速的通过参数或者配置文件形式操作 SWTC 链，实现转账，查询余额，多签名操作。

## Installation 安装

```bash
sudo npm install -g jcc-jingtum-tool --unsafe-perm=true
```

## wallet and configuration 钱包和配置

在用户的目录下存在.jcc-jingtum-tool/config.json 文件，类似配置

```javascript
{
  "server" : "https://srje115qd43qw2.swtc.top/",
  "wallet" : {"address": "jxxx", "secret": "sxxxx"}
}
```

**_注意：不能确认在安全情况下，不要在配置文件中使用明文保存密钥，尽量使用 keystore 文件_**

用户可以指定配置文件路径

```javascript
jcc-jingtum-tool --config myconfig.json
```

## normal operation 常规操作

- 创建钱包

```javascript
jcc-jingtum-tool --wallet_create
```

- 创建钱包并保存为 keystore 文件

```javascript
jcc-jingtum-tool --wallet_create --save_wallet
```

- 导入私钥存为 keystore 文件

```javascript
jcc-jingtum-tool --import_private_to_keystore
```

- 查询 rpc 节点信息

```javascript
jcc-jingtum-tool --server_info
```

- 查询区块

```javascript
jcc-jingtum-tool --ledger 22622884
jcc-jingtum-tool --ledger current
jcc-jingtum-tool --ledger closed
```

- 查询交易

```javascript
jcc-jingtum-tool --transaction AABBe089f12c9d4fcd82e47c3d3b56940c9ad6e51a9c7b5dfec4337f5fb4f58e
```

- 获取余额

```javascript
jcc-jingtum-tool --balance jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew
```

- 获取账号状态

```javascript
jcc-jingtum-tool --account_info jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew
```

- 转账

```javascript
# 从配置 (config.json) 的钱包向目的地址转账
jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.000001
jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.000001 --token JSLASH --memo "test"
```

- 创建多签名钱包

```javascript
# 创建多签名钱包
jcc-jingtum-tool --keystore jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy  --multi_create "2,jMETckC3Wtq2jAbrdHwbhCwLRxatboXrEt,1,jP3gCE8keCarT9Q25ceK3hJwhLv2wEG8Nv,1,jaLwe24yofQeejkNcBRJRsyk7Q9Y5mi2JA,1"

# 删除多签名钱包
jcc-jingtum-tool --keystore jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy  --multi_create "0"

# 通过多签的方式删除多签钱包
jcc-jingtum-tool --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy  --multi_create "0" --keystore jMETckC3Wtq2jAbrdHwbhCwLRxatboXrEt --save_sign s1.json

jcc-jingtum-tool --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy  --multi_create "0" --keystore jP3gCE8keCarT9Q25ceK3hJwhLv2wEG8Nv --save_sign s2.json

jcc-jingtum-tool --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy  --multi_create "0" --keystore jaLwe24yofQeejkNcBRJRsyk7Q9Y5mi2JA --save_sign s3.json

# 随后可以将s1.json s2.json s3.json文件名作为参数 执行 --multi_sign_commit

# 获取多签名状态
jcc-jingtum-tool --get_multi jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy

```

- 多签名转账

```javascript
# 多签名钱包生成签名内容
jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.000001 --token JSLASH --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jMETckC3Wtq2jAbrdHwbhCwLRxatboXrEt --save_sign s1.json

jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.000001 --token JSLASH --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jP3gCE8keCarT9Q25ceK3hJwhLv2wEG8Nv --save_sign s2.json

jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.000001 --token JSLASH --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jaLwe24yofQeejkNcBRJRsyk7Q9Y5mi2JA --save_sign s3.json

# 转SWT
jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.1 --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jMETckC3Wtq2jAbrdHwbhCwLRxatboXrEt --save_sign s1.json

jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.1 --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jP3gCE8keCarT9Q25ceK3hJwhLv2wEG8Nv --save_sign s2.json

jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.1 --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jaLwe24yofQeejkNcBRJRsyk7Q9Y5mi2JA --save_sign s3.json

# 多签名交易执行
jcc-jingtum-tool --multi_sign_commit "s1.json,s2.json,s3.json"
```

- 多签名启用禁用主密钥

```javascript
# 多签名钱包生成签名内容
jcc-jingtum-tool --disable_secret --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jMETckC3Wtq2jAbrdHwbhCwLRxatboXrEt --save_sign s1.json

jcc-jingtum-tool --disable_secret --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jP3gCE8keCarT9Q25ceK3hJwhLv2wEG8Nv --save_sign s2.json

jcc-jingtum-tool --disable_secret --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jaLwe24yofQeejkNcBRJRsyk7Q9Y5mi2JA --save_sign s3.json

# 转SWT
jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.1 --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jMETckC3Wtq2jAbrdHwbhCwLRxatboXrEt --save_sign s1.json

jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.1 --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jP3gCE8keCarT9Q25ceK3hJwhLv2wEG8Nv --save_sign s2.json

jcc-jingtum-tool --transfer jLyU8xB3D2VyjYkrVBU6XoW2Z9Qe9t2Xew --amount 0.1 --memo "test multi sign" --sign_for jH8kqWhBv2u4188gCvof6EK3EgQKRoKmGy --keystore jaLwe24yofQeejkNcBRJRsyk7Q9Y5mi2JA --save_sign s3.json

# 多签名交易执行
jcc-jingtum-tool --multi_sign_commit "s1.json,s2.json,s3.json"
```
