# Maintainer: Lance Chen <cyen0312+aur@gmail.com>

pkgbase=glance
pkgname=('glance-common' 'glance-api' 'glance-registry' 'python2-glance')
pkgver=2014.1.1
pkgrel=1
pkgdesc="OpenStack Image Registry and Delivery Service"
arch=(any)
url="https://launchpad.net/glance"
license=('Apache')
depends=('python2' 'python2-setuptools')
makedepends=('python2-setuptools')
options=('emptydirs')
source=("$url/icehouse/2014.1.1/+download/$pkgbase-$pkgver.tar.gz"
        "glance-api.service"
        "glance-registry.service")
md5sums=('43ce26d964589d851f2a21c8d070fca9'
         '13d487262de8951e20e3001ec0513c1b'
         '8ae81a07f547646b4fd4ecd89f596fda')

build() {
  cd "$pkgbase-$pkgver"
  /usr/bin/python2 setup.py build
  /usr/bin/python2 setup.py install --root="$srcdir/tmp" \
                                    --install-data="/" \
                                    --optimize=1
  cp -R etc/ "$srcdir/tmp/"
}

package_glance-common() {
  pkgdesc+=" - Common"
  install=glance-common.install
  depends=('python2-glance')

  cd tmp

  install -d "${pkgdir}/etc/glance/"
  install -m 640 etc/logging.cnf.sample "${pkgdir}/etc/glance/"

  install -d "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-control "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-manage "${pkgdir}/usr/bin/"

  install -d -m 0755 "${pkgdir}/var/lib/glance/"
  install -d -m 0755 "${pkgdir}/var/lib/glance/images/"
  install -d -m 0755 "${pkgdir}/var/lib/glance/image-cache/"
  install -d -m 0750 "${pkgdir}/var/log/glance/"
}

package_glance-api() {
  pkgdesc+=" - API"
  install=glance-api.install
  backup=('etc/glance/glance-api.conf'
          'etc/glance/glance-cache.conf'
          'etc/glance/glance-scrubber.conf')
  depends=('glance-common')

  cd tmp

  install -d "${pkgdir}/etc/glance/"
  install -m 640 etc/glance-api.conf "${pkgdir}/etc/glance/"
  install -m 640 etc/glance-api-paste.ini "${pkgdir}/etc/glance/"
  install -m 640 etc/glance-cache.conf "${pkgdir}/etc/glance/"
  install -m 640 etc/glance-scrubber.conf "${pkgdir}/etc/glance/"
  install -m 640 etc/policy.json "${pkgdir}/etc/glance/"
  install -m 640 etc/property-protections-policies.conf.sample "${pkgdir}/etc/glance/"
  install -m 640 etc/property-protections-roles.conf.sample "${pkgdir}/etc/glance/"
  install -m 640 etc/schema-image.json "${pkgdir}/etc/glance/"

  install -d "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-api "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-cache-cleaner "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-cache-prefetcher "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-cache-pruner "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-scrubber "${pkgdir}/usr/bin/"

  install -D -m 644 ${srcdir}/glance-api.service "${pkgdir}/usr/lib/systemd/system/glance-api.service"

  install -d -m 0755 "${pkgdir}/var/lib/glance/image-cache/incomplete/"
  install -d -m 0755 "${pkgdir}/var/lib/glance/image-cache/invalid/"
  install -d -m 0755 "${pkgdir}/var/lib/glance/image-cache/queue/"
}

package_glance-registry() {
  pkgdesc+=" - Registry"
  install=glance-registry.install
  backup=('etc/glance/glance-registry.conf')
  depends=('glance-common')

  cd tmp

  install -d "${pkgdir}/etc/glance/"
  install -m 640 etc/glance-registry.conf "${pkgdir}/etc/glance/"
  install -m 640 etc/glance-registry-paste.ini "${pkgdir}/etc/glance/"

  install -d "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-registry "${pkgdir}/usr/bin/"
  install -m 755 usr/bin/glance-replicator "${pkgdir}/usr/bin/"

  install -D -m 644 ${srcdir}/glance-registry.service "${pkgdir}/usr/lib/systemd/system/glance-registry.service"
}

package_python2-glance() {
  pkgdesc+=" - Python Library"
  install=python2-glance.install
  depends=('python2-pip')

  cd tmp

  install -d "${pkgdir}/usr/lib/"
  cp -R usr/lib/ "${pkgdir}/usr/"
}

# vim:set ts=2 sw=2 et:
