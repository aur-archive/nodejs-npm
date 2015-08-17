# Maintainer:   Guan 'kuno' Qing <neokuno at gmail dot com>
# Contributor:  tomxtobin <tomxtobin@tomxtobin.com>
# Contributor:  Marcus Carlsson <carlsson.marcus@gmail.com>
# Contributor:  jagoterr <jagoterr@gmail.com>
# Contributor:  hobarrera <hugo@osvaldobarrera.com.ar>
# Contributor:  Joshua Mendoza <joshua.mdza@gmail.com>

pkgname=nodejs-npm
pkgbasename=npm
pkgver=1.0.106
_realver="${pkgver}"
pkgrel=2
pkgdesc="a package manager for node"
arch=('any')
url="http://npmjs.org/"
license=('MIT')
groups=()
depends=('nodejs<0.6.3')
makedepends=()
optdepends=('bash-completion: for command line completion'
            'zsh: npm now supports zsh completion')
provides=()
conflicts=('nodejs-npm-git', 'nodejs>=0.6.3')
replaces=()
backup=('etc/npmrc')
options=(strip)
install=$pkgname.install
source=(http://registry.npmjs.org/npm/-/$pkgbasename-$_realver.tgz
$pkgname.install
npm.sh
npmrc)
noextract=()

build() {
  mkdir -p $pkgdir/etc/profile.d || return 1
  mkdir -p $pkgdir/usr/share/{doc,licenses/npm} || return 1

  export npm_config_userconfig=/tmp/npmrc || return 1
  export npm_config_globalconfig=/tmp/npmrc || return 1

  # Trun on temporary config for installation
  node $srcdir/package/cli.js config set unsafe-perm true || return 1
  node $srcdir/package/cli.js config set prefix $pkgdir/usr || return 1

  # Installation
  node $srcdir/package/cli.js install -g ./$pkgbasename-$_realver.tgz || return 1

  # Trun off temporary config
  node $srcdir/package/cli.js config set prefix /usr || return 1
  node $srcdir/package/cli.js config set unsafe-perm false || return 1
}

package() {
  # Set global npm config file to /etc/npmrc
  install -m644 $srcdir/npmrc $pkgdir/etc/npmrc || return 1

  # Tell shell set npm global config file to /etc/npmrc
  install -m644 $srcdir/npm.sh $pkgdir/etc/profile.d/npm.sh || return 1

  # Output shell completion function
  #node $srcdir/package/cli.js completion >> $pkgdir/etc/profile.d/npm.sh || return 1

  # Install html document
  cp -r $srcdir/package/html $pkgdir/usr/share/doc/npm || return 1

  # Install license
  install -m644 $srcdir/package/LICENSE $pkgdir/usr/share/licenses/npm || return 1
}
md5sums=('44f82461713f911d9a01f194bdc891bd'
         '8091034503584c099cbef79b74b2ddd1'
         '47e347c31ddcee254fdb47e772d0994f'
         'f23089c429100dafa0c8a610429088ca')
