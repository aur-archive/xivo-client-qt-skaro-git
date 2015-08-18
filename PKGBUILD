# Maintainer: Benoit Stahl <stahlbenoit@gmail.com>

pkgname='xivo-client-qt-skaro-git'
pkgver=20130119
pkgrel=1
pkgdesc="XiVO client QT"
arch=('i686' 'x86_64')
url='http://wiki.xivo.fr'
license=('GPL3')
depends=('qt')
makedepends=('git' 'upx')
provides=('xivo-client')
conflicts=()
#source=(xivo-client)
#md5sums=('ad53290a0740b49d04e6527329435b5a')
_gitroot="git://git.xivo.fr/official/xivo-client-qt.git"
_gitname="xivo-client-qt"

build() {
	msg "Connecting to the GIT server...."
	if [[ -d ${srcdir}/${_gitname} ]] ; then
		cd ${srcdir}/${_gitname}
		git pull origin
		msg "The local files are updated..."
	else
		git clone ${_gitroot} --depth 1
	fi
	msg "GIT checkout done."
 
	cd ${srcdir}/${_gitname}/
	qmake
	make
}

package() {

	mkdir -p ${pkgdir}/opt/xivoclient/plugins
	mkdir -p ${pkgdir}/usr/bin
	mkdir -p ${pkgdir}/usr/share/icons/xivoclient
	install -D -m 755 -o root -g root ${srcdir}/${_gitname}/bin/xivoclient ${pkgdir}/opt/xivoclient/
	install -D -m 755 -o root -g root ${srcdir}/${_gitname}/bin/libxivoclient* ${pkgdir}/opt/xivoclient/
	install -D -m 755 -o root -g root ${srcdir}/${_gitname}/bin/plugins/* ${pkgdir}/opt/xivoclient/plugins/
	install -D -m 655 -o root -g root ${srcdir}/${_gitname}/packaging/resources/xivoclient.png ${pkgdir}/usr/share/icons/xivoclient.png
	install -D -m 655 -o root -g root ${srcdir}/${_gitname}/packaging/resources/xivoclient.desktop ${pkgdir}/usr/share/applications/xivoclient.desktop
	
		cat >${pkgdir}/usr/bin/xivoclient <<EOF
#!/bin/bash

DEBUG="no"

while getopts ":dh" opt; do
  case \$opt in
      d)
          DEBUG="yes"
          ;;
      h)
         echo "Usage : \$0 [-dh] [profile]"
         echo
         echo "-d : Enable debug output"
         echo "-h : Help"
         echo "profile : Configuration profile"
         echo
         exit 0
         ;;
     \?)
         echo "Invalid option: -\$OPTARG" >&2
         ;;
  esac
done

shift \$(( OPTIND-1 ))

cd /opt/xivoclient

if [ "\$DEBUG" = "yes" ]
then
    LD_LIBRARY_PATH=".:\$LD_LIBRARY_PATH" ./xivoclient \$@
else
    LD_LIBRARY_PATH=".:\$LD_LIBRARY_PATH" ./xivoclient \$@ >& /dev/null
fi	
EOF
	chmod 775 ${pkgdir}/usr/bin/xivoclient 
}

