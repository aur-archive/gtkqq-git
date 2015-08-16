# Maintainer: cuihao <cuihao dot leo at gmail dot com>

pkgname=gtkqq-git
pkgver=20120301
pkgrel=1
pkgdesc="A QQ client based on gtk+ uses WebQQ protocol."
url="http://code.google.com/p/gtk-qq/"
license=('GPL3')
arch=('x86_64' 'i686')
depends=('gtk2' 'bzip2' 'sqlite3' 'libnotify' 'gstreamer0.10')
makedepends=('git')
conflicts=('linuxqq')

_gitname="gtkqq"
_gitroot="git://github.com/kernelhcy/gtkqq.git"
_gitbranch="dev"

build() {
    cd $srcdir
    msg "Connecting to the GIT server...."
  
    if [ -d $_gitname ]; then
        cd $_gitname && git pull origin && cd ..
        msg "The local files are updated."
    else
        git clone $_gitroot $_gitname -b $_gitbranch
    fi
    
    msg "GIT checkout done"
    
    rm -rf "$_gitname-build"
    cp -rf "$_gitname" "$_gitname-build"
    cd "$_gitname-build"
    # test部分编译出来的东西貌似没啥用，割不割呢？
    sed -i "s/test//" src/Makefile.am
    # 禁用-Werror编译参数，会导致编译错误。要是编译错误就去掉。
    #sed -i "s/-Werror//" src/test/Makefile.am
    #sed -i "s/-Werror//" src/libqq/Makefile.am
    #sed -i "s/-Werror//" src/gui/Makefile.am
    
    msg "Starting configure..."
    ./autogen.sh
    ./configure --prefix=/usr --disable-debug --enable-gst --disable-nm 
    
    msg "Starting make..."
    make
    make DESTDIR=${pkgdir}/ install
}

package() {
    rm -f ${pkgdir}/usr/bin/qq
}

