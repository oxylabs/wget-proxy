# How to Use Wget With Proxy

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

[![](https://dcbadge.vercel.app/api/server/eWsVUJrnG5)](https://discord.gg/GbxmdGhZjq)

[<img src="https://img.shields.io/static/v1?label=&message=wget&color=brightgreen" />](https://github.com/topics/wget)

- [How to install Wget](#how-to-install-wget)
- [Running Wget](#running-wget)
- [Downloading a single file](#downloading-a-single-file)
- [Changing the User-Agent](#changing-the-user-agent)
- [Downloading multiple files](#downloading-multiple-files)
- [Extracting links from a webpage](#extracting-links-from-a-webpage)
- [Using proxies with Wget](#using-proxies-with-wget)

Wget is a popular command-line utility that can download files from the web. It’s part of the GNU Project and, as a result, commonly bundled with numerous Linux distributions.

This article will provide you with an overview of this utility.

For a detailed explanation, see our [blog post](https://oxylabs.io/blog/wget-proxy).

## How to install Wget

Wget can be downloaded from the [official GNU channel](https://www.gnu.org/software/wget/) and installed manually.

To install Wget on Ubuntu/Debian, open the terminal and run the following command:

```shell
sudo apt-get install wget
```

To install Wget on CentOS/RHEL, open the terminal and run the following command:

```shell
yum install wget
```

If you’re using macOS, we highly recommend using the [Homebrew](https://brew.sh/) package manager. Open the terminal and run the following command:

```shell
brew install wget
```

If you’re using Windows, [Chocolatey](https://chocolatey.org/) package manager is a good choice. When using Chocolatey, run the following command from the command line or PowerShell:

```powershell
choco install wget
```

Lastly, to verify the installation of Wget, run the following command:

```shell
wget --version
```

## Running Wget

Open the terminal and enter the following:

```
wget -h
```

This will list all the options used with the Wget command grouped in categories, such as Startup, Logging, Download, etc.

### Downloading a single file

To download a single file, run Wget and type in the complete URL of the file:

```
wget https://ftp.gnu.org/gnu/wget/wget2-2.0.0.tar.lz
```

![](https://images.prismic.io/oxylabs-sm/ZGI5NGQ5ZjctMWZkNS00YjliLWFlMTYtNWQ4ZGI5MDE5Yjk2_wget_single_file.png?auto=compress,format&rect=0,0,1115,402&w=1115&h=402&fm=webp&q=75)

### Changing the User-Agent

To identify the User-Agent used by Wget, request this URL:

```shell
wget https://httpbin.org/user-agent
```

This command will download a file named user-agent without any extension. To view the contents of this file, use the `cat` command:

```shell
~$ cat user-agent
{
  "user-agent": "wget/1.21.2"
}
```

The default User-Agent can be modified using the --header option:

```shell
wget --header "user-agent: DESIRED USER AGENT" URL-OF-FILE
```

The following example should clarify it further:

```shell
~$ wget  --header "user-agent: Mozilla/5.0 (Macintosh)" https://httpbin.org/user-agent
~$ cat user-agent
{
  "user-agent": "Mozilla/5.0 (Macintosh)"
}
```

### Downloading multiple files

The following command will download files from all three URLs:

```
~$ wget http://example.com/file1.zip http://example.com/file2.zip http://example.com/file3.zip
```

**The second method** is to write all the URLs in a file and use the `-i` or `--input-file` option:

```
~$ wget --input-file=urls.txt
~$ wget -i urls.txt
```

### Extracting links from a webpage

You can supply a URL that contains the links to the files:

```shell
~$ wget --input-file=https://ftp.gnu.org/gnu/wget
```

To download all files with a `.sig` extension, use the following command:

```shell
~$ wget --recursive --no-parent --no-directories --no-clobber --accept=sig --input-file=https://ftp.gnu.org/gnu/wget
```

## Using proxies with Wget

The first method uses command line switches to specify the proxy server and authentication details.

First, check your current IP address. Run Wget in quiet mode and redirect the output to the terminal instead of downloading the file:

```shell
~$ wget --quiet --output-document=- https://ip.oxylabs.io/location
# OR
~$ wget -q -O - https://ip.oxylabs.io/location
```

To use a proxy that doesn’t require authentication, use two `-e` or two `--execute` switches:

```shell
~$ wget -q -O- -e use_proxy=yes -e http_proxy=12.13.14.15:1234 https://ip.oxylabs.io/location
```
Output:
```json
{"ip":"104.200.141.20","providers":{"dbip":{"country":"US","asn":"AS46562","org_name":"Performive LLC","city":"New York","zip_code":"","time_zone":"","meta":"\u003ca href='https://db-ip.com'\u003eIP Geolocation by DB-IP\u003c/a\u003e"},"ip2location":{"country":"US","asn":"","org_name":"","city":"New York City","zip_code":"10011","time_zone":"-05:00","meta":"This site or product includes IP2Location LITE data available from \u003ca href=\"https://lite.ip2location.com\"\u003ehttps://lite.ip2location.com\u003c/a\u003e."},"ipinfo":{"country":"US","asn":"AS46562","org_name":"Performive LLC","city":"","zip_code":"","time_zone":"","meta":"\u003cp\u003eIP address data powered by \u003ca href=\"https://ipinfo.io\" \u003eIPinfo\u003c/a\u003e\u003c/p\u003e"},"maxmind":{"country":"US","asn":"AS46562","org_name":"PERFORMIVE","city":"","zip_code":"","time_zone":"-06:00","meta":"This product includes GeoLite2 Data created by MaxMind, available from https://www.maxmind.com."}}}
```

If the proxy server requires user authentication, set the proxy username by using the` --proxy-user` switch. Similarly, set the proxy password using the` --proxy-password` switch:

```shell
~$ wget -q -O- -e use_proxy=yes -e http_proxy=12.13.14.15:1234  --proxy-user=your_username --proxy-password=your_password https://ip.oxylabs.io/location
```

**The second method** is to use the .wgetrc configuration file.

In the ~/.wgetrc file, enter the following lines:

```shell
use_proxy = on
http_proxy = http://12.13.14.15:1234
```

If you also need to set user authentication for the proxy, modify the file as follows:

```shell
use_proxy = on
http_proxy = http://your_username:your_password@12.13.14.15:1234
```

As of now, every time Wget runs, it’ll use the specified proxy.

```shell
$ wget -q -O- https://ip.oxylabs.io
# Prints IP of the proxy server
```

If you wish to learn more about wget, see our [blog post](https://oxylabs.io/blog/wget-proxy).
