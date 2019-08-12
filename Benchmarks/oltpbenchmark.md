# OLTPBenchamrk Setup
> OLTPBenchmark 설정방법에 대해 알아본다.

## Install

### Prerequisite
* Java (v1.7 or newer)
* Apache Ant 
* [Apache Ivy](https://ant.apache.org/ivy/) (dependency 설치 위해 필요함.)
* mysql-connector-java (For MySQL)

~~~
# Download the project from GitHub Repository
git clone https://github.com/oltpbenchmark/oltpbench.git
cd oltpbench
# 프로젝트 빌드를 위해서는 Ivy가 필요함.
ant bootstrap
# 해당 프로젝트 빌드를 위한 dependency 설치함.
ant resolve 
ant build 
~~~
---

## Configuration
* config 디렉토리에 각각의 benchmark에 대한 설정 파일이 있다. (e.g., `sample_tpcc_config.xml`)
~~~
<!-- config/sample_tpcc_config.xml -->
    <!-- Connection details -->
    <dbtype>mysql</dbtype>
    <driver>com.mysql.jdbc.Driver</driver>
    <DBUrl>jdbc:mysql://localhost:3306/tpcc</DBUrl>
    <username>root</username>
    <password>mysecretpassword</password>
    <isolation>TRANSACTION_SERIALIZABLE</isolation>
~~~
* `<dbtype>` 성능을 평가할 데이터베이스 엔진을 명시한다. 해당 데이터베이스들은 jdbc 호환이 가능해야함.
* `<driver>` jdbc driver를 명시함.
* `<DBUrl>` 보통, localhost와 `3306` port를 사용하며, 데이터베이스 이름을 명시해야 한다.
* `<isolation>` 데이터베이스 엔진에서 제공하는 트랜잭션 동시성 제어 레벨을 명시함.

~~~
     TRANSACTION_READ_UNCOMMITTED
     TRANSACTION_READ_COMMITTED
     TRANSACTION_REPEATABLE_READ
     TRANSACTION_SERIALIZABLE
~~~


## Load
* [TPC-C](http://www.tpc.org/tpcc/)
~~~
./oltpbenchmark -b tpcc -c config/sample_tpcc_config.xml --create=true --load=true
~~~

* [CH-Benchmark](https://db.in.tum.de/research/projects/CHbenCHmark/?lang=en)
~~~
./oltpbenchmark -b tpcc,chbenchmark -c config/mix_config_mysql.xml --create=true --load=true
~~~

* CH-Benchmark를 수행하기 위해서는 tpcc로딩을 함께 해야한다. 따라서, `mix_config_mysql.xml`을 사용하면된다.

## Run
* [TPC-C](http://www.tpc.org/tpcc/)
~~~
./oltpbenchmark -b tpcc -c config/sample_tpcc_config.xml --execute=true -s 1 -o outputfile
~~~

* [CH-Benchmark](https://db.in.tum.de/research/projects/CHbenCHmark/?lang=en)

~~~
./oltpbenchmark -b chbenchmark,tpcc -c config/mix_config_mysql.xml --execute=true -s 1 -o outputfile
~~~

* `-s` 옵션은 결과값 sampling window 갯수를 조절할 수 있음.
* `outputfile`에 Benchmark 수행결과가 명시됨.


## Output
* Benchmark 수행결과는 `results` 폴더에 저장된다. 
* `-o` 옵션의 파일명을 기반으로 저장됨.
* `-s 1` 옵션을 사용하면 TPS (초당 트랜잭션 횟수)를 구할 수 있음.
~~~
time (seconds),throughput (requests/s),average,min,25th,median,75th,90th,95th,99th,max
0,200.200,1.183,0.585,0.945,1.090,1.266,1.516,1.715,2.316,12.656
5,199.800,0.994,0.575,0.831,0.964,1.071,1.209,1.424,2.223,2.657
10,200.000,0.984,0.550,0.796,0.909,1.029,1.191,1.357,2.024,35.835
~~~

## Trouble Shooting

### Table 존재하지 않음 에러
* CH-Benchmark를 수핸하는 도중 아래와 같은 에러가 발생하면서 동작이 안되는 경우가 발생한다.
~~~
Table tpcc.xxx does not exists ...
~~~

* 테이블 로딩은 제대로 되었지만, MySQL data 디렉토리를 보면 테이블명이 모두 **대문자**로 저장되어 있다.
따라서 `my.cnf`에 테이블명 대소문자 구분을 하지 않는 옵션을 추가한 후 다시 로딩을 하면 된다.
~~~
lower_case_table_names = 1
~~~

## Help 
~~~
$ ./oltpbenchmark --help
usage: oltpbenchmark
-b,--bench <arg>             [required] Benchmark class. Currently
                             supported: [tpcc, tatp, wikipedia,
                             resourcestresser, twitter, epinions, ycsb,
                             jpab, seats, auctionmark]
-c,--config <arg>            [required] Workload configuration file
   --clear <arg>             Clear all records in the database for this
                             benchmark
   --create <arg>            Initialize the database for this benchmark
   --dialects-export <arg>   Export benchmark SQL to a dialects file
   --execute <arg>           Execute the benchmark workload
-h,--help                    Print this help
   --histograms              Print txn histograms
   --load <arg>              Load data using the benchmark's data loader
-o,--output <arg>            Output file (default System.out)
   --runscript <arg>         Run an SQL script
-s,--sample <arg>            Sampling window
-v,--verbose                 Display Messages
~~~
