# ip

A SDK for ip.

[![Build Status](https://travis-ci.com/DuC-cnZj/ip.svg?branch=master)](https://travis-ci.com/DuC-cnZj/ip)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/DuC-cnZj/ip/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/DuC-cnZj/ip/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/DuC-cnZj/ip/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/DuC-cnZj/ip/?branch=master)
![StyleCI build status](https://github.styleci.io/repos/156356126/shield)

## Support

- [baidu api](http://lbsyun.baidu.com/index.php?title=webapi/ip-api)
- taobao api
- [ali api](https://market.aliyun.com/products/57002002/cmapi018957.html?spm=5176.730005.productlist.d_cmapi018957.6f613524XYMOwf&innerSource=search_ip#sku=yuncode1295700000)
- [tencent api](https://lbs.qq.com/webservice_v1/guide-ip.html)

## Installing

```shell
composer require duc_cnzj/ip
```

## Usage

```php
$client = new IpClient();

$client->setIp('xxx.xxx.xxx.xxx')->getCity();
$client->setIp('xxx.xxx.xxx.xxx')->getCountry();
$client->setIp('xxx.xxx.xxx.xxx')->getRegion();
$client->setIp('xxx.xxx.xxx.xxx')->getIp();

# or
$client->setIp('xxx.xxx.xxx.xxx');
$client->getCity();
$client->city;

# use Provider (baidu, taobao, ali)
$client = new IpClient();

# if use baidu api, you need provide ak secret.
$client->useProvider('baidu')
    ->setProviderConfig('baidu', ['ak' => 'xxxxxxxxxxxx']);
# or 
$client->useProvider('baidu')
    ->setProviderConfig('baidu', 'xxxxxxxxxxxx');

# if use tencent api, you need provide ak secret.
$client->useProvider('tencent')
    ->setProviderConfig('tencent', ['key' => 'xxxxxxxxxxxx']);
# or 
$client->useProvider('tencent')
    ->setProviderConfig('tencent', 'xxxxxxxxxxxx');

# mutil set/get configs
$client->setConfigs(['ali' => 'xxxxx', 'baidu' => 'xxxxx']);
$client->getConfigs('ali', 'baidu');

# if use ali api, you need provide ak secret.
$client->useProvider('ali')
    ->setProviderConfig('ali', ['app_code' => 'xxxxxxxxxxxx']);

# if use taobao.
$client->useProvider('taobao');

# default use all provider. if you want use all, pls set secret for each provider.
$client->setProviderConfig('baidu', ['ak' => 'xxxxxxxxxxxx']);
$client->setProviderConfig('ali', 'xxxxxxxxxxxx');

# package will try 3 times when provider response error. use try method to reset tryTimes.
$client->setIp('xxx.xxx.xxx.xxx')->try(10);

# in laravel
 $client = app('ip');
```

extra property
```php
# baidu/ali/tencent return point_x and point_y, so you can:
$client->useProvider('baidu')
    ->setProviderConfig('baidu', ['ak' => 'xxxxxxxxxxxx']);

$client->useProvider('ali')
    ->setProviderConfig('ali', ['app_code' => 'xxxxxxxxxxxx']);

$client->useProvider('tencent')
    ->setProviderConfig('tencent', 'xxxxxxxxxxxx');

$client->getPointX();
$client->getPointY();

# taobao return isp, so u can:

$client->useProvider('taobao')
    ->setIp('xxx.xxx.xxx.xxx')
    ->getIsp();
```

when getXXX() false it will return null, u can use `$client->getErrors()` get error info;

alias
```php
$client->use('taobao', 'baidu'); # It will be executed in the order you passed it in
# Equals to
$client->useProvider('taobao', 'baidu');
```

other methods
```php
$client->clearUse(); # will clear providers;

# custom your instance. need implement DucCnzj\Ip\Imp\IpImp, Object or String.
# if it dont need secret, pls set the third parameter false;
$client->bind('taobao', $taobao);

# set custom CacheStore. default is ArrayStore. need implement DucCnzj\Ip\Imp\CacheStoreImp;
$client->setCacheStore($store);
```

## License

MIT
