# Maintainer:Bjoern Bidar <theodorstormgrade@gmail.com>
#_gui_toolkit=qt # qt or gtk
_build_server=false # set true to build server or both build both 
_CMAKE_ARGS=('-DWITH_STATIC=OFF' '-DWITH_NEL_TOOLS=OFF' '-DWITH_NEL_TESTS=OFF' '-DWITH_NEL_SAMPLES=OFF' '-DWITH_RYZOM_TOOLS=OFF'.)
pkgname=ryzom-client-latest-hg
pkgver=20121017
pkgrel=2
pkgdesc="Ryzom is a Pay to Play MMORPG with a free demo. This version is for playing on an official server"
arch=('i686' 'x86_64')
url="http://www.ryzom.com/"
license=('AGPL3')
depends=('curl' 'freetype2' 'libx11' 'mesa' 'libxxf86vm' 'openal' 'freealut' 'libogg' 'libvorbis' 'libxml2' 'cmake' 'libpng' 'libjpeg' 'rrdtool' 'bison' 'libwww' 'boost' 'cpptest' 'luabind' 'libsquish' 'lua'  'luasql-mysql') 
case $_build_server in
  true) _CMAKE_ARGS=( ${_CMAKE_ARGS[*]} '-DWITH_RYZOM_SERVER=ON' '-DWITH_RYZOM_CLIENT=OFF') ;;
  false) _CMAKE_ARGS=( ${_CMAKE_ARGS[*]} '-DWITH_RYZOM_SERVER=OFF' ) depends=( ${depends[*]} 'ryzom-data' );;
  both) _CMAKE_ARGS=( ${_CMAKE_ARGS[*]} '-DWITH_RYZOM_SERVER=ON' '-DWITH_RYZOM_CLIENT=ON') depends=( ${depends[*]} 'ryzom-data' ) ;;
esac 

makedepends=('mercurial')
provides=('ryzom')
source=( 'ryzom.sh' 'ryzom.desktop' )
md5sums=( 'a5ca7dfae7b9073f78cd1b0b7380755f' '71d5136d40ec4e76c2ac2b0c9e506aef')


case $_gui_toolkit in
  qt) _CMAKE_ARGS=( ${_CMAKE_ARGS[*]} '-DWITH_QT=ON' ) ;;
  gtk) _CMAKE_ARGS=( ${_CMAKE_ARGS[*]} '-DWITH_GTK=ON' ) ;;
esac 


_hg_root='http://ryzom.hg.sourceforge.net:8000/hgroot/ryzom/ryzom'
_hg_name='ryzom' 


build() {
    
    if [ -d "$_hg_name" ] ; then
      cd "$_hg_name"
      hg pull && hg update
      cd ..
    else
      hg clone "$_hg_root"
    fi
    
    cd $_hg_name
    mkdir -p build
    cd build

    msg "hg clone done or server timeout"
    msg "Starting cmake..."
    cmake -b ../code  ${_CMAKE_ARGS[*]} -DCMAKE_INSTALL_PREFIX=/usr -DRYZOM_ETC_PREFIX=/etc/ryzom -DRYZOM_SHARE_PREFIX=/usr/share/ryzom -DRYZOM_BIN_PREFIX=/usr/bin -DRYZOM_GAMES_PREFIX=/usr/bin 

    

    msg "Starting make"   
    make -j12
}

package() {
    cd "$srcdir/$_hg_name/build"
    make DESTDIR="$pkgdir/" install
    install -m 644 ${srcdir}/ryzom.desktop ${pkgdir}/usr/share/applications
    sed -ie 's/\/usr\/bin\/ryzom_client/ryzom/' ${pkgdir}/usr/share/applications/ryzom.desktop # replace ryzom_client with our script
  #  mv ${pkgdir}/usr/games/* ${pkgdir}/usr/bin
    cp ${srcdir}/ryzom.sh  ${pkgdir}/usr/bin/ryzom
    chmod +x ${pkgdir}/usr/bin/ryzom
    mkdir -p ${pkgdir}/usr/bin
   
}
 



