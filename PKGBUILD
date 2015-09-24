# Maintainer: Nicolas Leclercq <nicolas.private@gmail.com>

pkgname='telegraf'
pkgver='0.1.9'
pkgrel='1'
epoch=
pkgdesc='Server-level metric gathering agent for InfluxDB'
arch=('i686' 'x86_64')
url='http://influxdb.org/'
license=('MIT')
groups=()
depends=()
makedepends=('go' 'git')
checkdepends=()
optdepends=()
provides=('telegraf')
conflicts=()
replaces=()
backup=('etc/telegraf/telegraf.conf')
options=()
install="$pkgname.install"
pkgtar="v${pkgver}.tar.gz"
source=("https://github.com/influxdb/telegraf/archive/$pkgtar"
        "$pkgname.install")
changelog=
noextract=()
md5sums=('ae8312d37649bd2538c50c6f9f9d8455'
         'SKIP')

prepare()
{ 
  export GOPATH="${srcdir}"
  export GOBIN="$GOPATH/bin"
  export TELEGRAFPATCH="$GOPATH/src/github.com/influxdb/telegraf"
  if [ -d $GOBIN ]; then
    rm -rf $GOBIN;
  fi;

  echo "Extracting telegraf archive..."
  mkdir -p $TELEGRAFPATCH
  tar -C $TELEGRAFPATCH --strip-components=1 -xzf $pkgtar

}
build() 
{
  echo "Building $pkgname ..."
  cd "$GOPATH/src/github.com/influxdb/telegraf"
  make
}
package()
{
  # systemctl service file
  install -D -m644  "$GOPATH/src/github.com/influxdb/telegraf/scripts/telegraf.service" "$pkgdir/usr/lib/systemd/system/telegraf.service"
  sed -i 's;/etc/opt/telegraf;/etc/telegraf;g' "$pkgdir/usr/lib/systemd/system/telegraf.service"
  sed -i 's;/opt/telegraf;/usr/bin;g' "$pkgdir/usr/lib/systemd/system/telegraf.service"

  # binaries
  install -D -m755 "$GOPATH/src/github.com/influxdb/telegraf/telegraf" "$pkgdir/usr/bin/telegraf"

  # configuration file
  mkdir -p "$pkgdir/etc/telegraf"
  $GOPATH/src/github.com/influxdb/telegraf/telegraf -sample-config > "$pkgdir/etc/telegraf/telegraf.conf"

  # license
  install -Dm644 "$GOPATH/src/github.com/influxdb/telegraf/LICENSE" "$pkgdir/usr/share/licenses/telegraf/LICENSE"
}
