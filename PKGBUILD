# Maintainer: Michael Spradling <mike@mspradling.com>
# Contributor: Daniel Albers <daniel@lbe.rs>

pkgbase='bareos'

pkgname=("${pkgbase}-common"
         "${pkgbase}-bconsole"
         "${pkgbase}-database-common"
         "${pkgbase}-database-mysql"
         "${pkgbase}-database-postgresql"
         "${pkgbase}-database-sqlite3"
         "${pkgbase}-database-tools"
         "${pkgbase}-devel"
         "${pkgbase}-director"
         "${pkgbase}-storage-fifo"
         "${pkgbase}-tools"
         "${pkgbase}-filedaemon"
         "${pkgbase}-storage"
         "${pkgbase}-storage-tape"
         "${pkgbase}-director-python-plugin"
         "${pkgbase}-filedaemon-python-plugin"
         "${pkgbase}-storage-python-plugin")

pkgver=17.2.4rc2
_pkgver=17.2.4
pkgrel=1
arch=(i686 x86_64)
groups=('bareos')
pkgdesc="Bareos - Backup Archiving REcovery Open Sourced"
url="http://www.bareos.org"
license=('AGPL3')
source=("git+https://github.com/bareos/bareos.git#tag=Release/$_pkgver-rc2")
md5sums=('SKIP')

prepare() {
  cd $pkgbase
  sed -i "s/python-config/python2-config/g" configure
}

build() {
  cd $pkgbase
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --localstatedir=/ \
    --libdir=/usr/lib/$pkgbase \
    --disable-conio \
    --enable-dynamic-cats-backends \
    --enable-dynamic-storage-backends \
    --with-python \
    --with-openssl \
    --with-mysql \
    --disable-rpath \
    --with-postgresql \
    --with-sqlite3 \
    --with-logdir=/var/log/$pkgbase \
    --with-dir-user=$pkgbase \
    --with-dir-group=$pkgbase \
    --with-sd-user=$pkgbase \
    --with-sd-group=$pkgbase \
    --with-fd-user=root \
    --with-pid-dir=/run/bareos \
    --with-fd-group=root \
    --with-configtemplatedir=/usr/share/$pkgbase/config \
    --with-working-dir=/var/lib/$pkgbase \
    --with-plugindir=/usr/lib/$pkgbase/plugins \
    --with-scriptdir=/usr/lib/$pkgbase/scripts \
    --with-backenddir=/usr/lib/$pkgbase/backends \
    --with-archivedir=/var/lib/$pkgbase/storage \
    --with-bsrdir=/var/lib/$pkgbase \
    --with-subsys-dir=/run/$pkgbase \
    --with-systemd && make
    #TODO systemd files

    make DESTDIR="$srcdir/install" install install-autostart
}


_cp() {
  mkdir -p "$(dirname $2)"
  cp -a "$1" "$2"
}

package_bareos-common() {
  # Matches bareos-common.install.in
  pkgdesc="${pkgdesc} - Common files"
  depends=('libcap' 'openssl' 'acl' 'bash' 'lzo' 'jansson' 'zlib')
  install=bareos-common.install

  for f in \
    usr/lib/bareos/libbareos-$_pkgver.so \
    usr/lib/bareos/libbareos.so \
    usr/lib/bareos/libbareoscfg-$_pkgver.so \
    usr/lib/bareos/libbareoscfg.so \
    usr/lib/bareos/libbareosfind-$_pkgver.so \
    usr/lib/bareos/libbareosfind.so \
    usr/lib/bareos/scripts/bareos-config \
    usr/lib/bareos/scripts/bareos-config-lib.sh \
    usr/lib/bareos/scripts/btraceback.gdb \
    usr/bin/bsmtp \
    usr/bin/btraceback \
    usr/share/man/man1/bsmtp.1.gz \
    usr/share/man/man8/btraceback.8.gz \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done

  install -d $pkgdir/var/lib/bareos
  install -d $pkgdir/etc/bareos
  install -d $pkgdir/run/bareos
}

package_bareos-bconsole() {
  pkgdesc="${pkgdesc} - Admin Tool (CLI)"
  depends=("bareos-common=$pkgver" 'readline' 'jansson')
  groups+=('bareos-client')
  install=bareos-bconsole.install

  for f in \
    /usr/share/bareos/config/bconsole.conf \
    usr/bin/bconsole \
    usr/share/man/man1/bconsole.1.gz \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-database-common() {
  pkgdesc="${pkgdesc} - Generic abstration libs and tools for sql catalog"
  depends=("bareos-common=$pkgver" 'libcap' 'lzo' 'zlib' 'openssl' 'bash' 'jansson')

  for f in \
    usr/lib/bareos/libbareoscats-$_pkgver.so \
    usr/lib/bareos/libbareoscats.so \
    usr/lib/bareos/libbareossql-$_pkgver.so \
    usr/lib/bareos/libbareossql.so \
    usr/lib/bareos/scripts/create_bareos_database \
    usr/lib/bareos/scripts/drop_bareos_database \
    usr/lib/bareos/scripts/drop_bareos_tables \
    usr/lib/bareos/scripts/grant_bareos_privileges \
    usr/lib/bareos/scripts/make_bareos_tables \
    usr/lib/bareos/scripts/update_bareos_tables \
    usr/lib/bareos/scripts/ddl/versions.map \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-database-mysql() {
  pkgdesc="${pkgdesc} - Libs and tools for mysql catalog"
  provides=("bareos-database=$pkgver")
  depends=("bareos-database-common=$pkgver" 'libmariadbclient')
  install=bareos-database-mysql.install

  for f in \
    usr/lib/bareos/backends/libbareoscats-mysql-$_pkgver.so \
    usr/lib/bareos/backends/libbareoscats-mysql.so \
    usr/lib/bareos/scripts/ddl/drops/mysql.sql \
    usr/lib/bareos/scripts/ddl/grants/mysql.sql \
    usr/lib/bareos/scripts/ddl/creates/mysql.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.10_11.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.11_12.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.12_14.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.14_2001.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.2001_2002.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.2002_2003.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.2003_2004.sql \
    usr/lib/bareos/scripts/ddl/updates/mysql.2004_2171.sql \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-database-postgresql() {
  pkgdesc="${pkgdesc} - Libs and tools for postgresql catalog"
  provides=("bareos-database=$pkgver")
  depends=("bareos-database-common=$pkgver" "postgresql-libs")
  install='bareos-database-postgresql.install'

  for f in \
    usr/lib/bareos/backends/libbareoscats-postgresql-$_pkgver.so \
    usr/lib/bareos/backends/libbareoscats-postgresql.so \
    usr/lib/bareos/scripts/ddl/drops/postgresql.sql \
    usr/lib/bareos/scripts/ddl/grants/postgresql-change_owner.dbconfig-template.sql \
    usr/lib/bareos/scripts/ddl/grants/postgresql-readonly.sql \
    usr/lib/bareos/scripts/ddl/grants/postgresql.sql \
    usr/lib/bareos/scripts/ddl/creates/postgresql.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.10_11.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.11_12.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.12_14.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.14_2001.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.2001_2002.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.2002_2003.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.2003_2004.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.2004_2171.sql \
    usr/lib/bareos/scripts/ddl/updates/postgresql.bee.1017_2004.sql \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-database-sqlite3() {
  pkgdesc="${pkgdesc} - Libs and tools for sqlite3 catalog"
  provides=("bareos-database=$pkgver")
  depends=("bareos-database-common=$pkgver" "sqlite3")

  for f in \
    usr/lib/bareos/backends/libbareoscats-sqlite3-$_pkgver.so \
    usr/lib/bareos/backends/libbareoscats-sqlite3.so \
    usr/lib/bareos/scripts/ddl/creates/sqlite3.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.10_11.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.11_12.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.12_14.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.14_2001.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.2001_2002.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.2002_2003.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.2003_2004.sql \
    usr/lib/bareos/scripts/ddl/updates/sqlite3.2004_2171.sql \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-database-tools() {
  pkgdesc="${pkgdesc} - CLI tools with database dpendencies (dbcheck, bscan)"
  depends=("bareos-database-common=$pkgver" 'openssl' 'jansson')

  for f in \
    usr/bin/bareos-dbcheck \
    usr/bin/bscan \
    usr/share/man/man8/bareos-dbcheck.8.gz \
    usr/share/man/man8/bscan.8.gz \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-devel() {
  pkgdesc="${pkgdesc} - Devel headers"
  arch=(any)
  # TODO
  install -d $pkgdir/usr/include/bareos
}

package_bareos-director() {
  pkgdesc="${pkgdesc} - Director daemon"
  depends=("bareos-common=$pkgver" 'openssl' 'bash' 'jansson')
  install=bareos-director.install

  for f in \
    usr/lib/bareos/scripts/delete_catalog_backup \
    usr/lib/bareos/scripts/make_catalog_backup \
    usr/lib/bareos/scripts/make_catalog_backup.pl \
    usr/lib/bareos/scripts/query.sql \
    usr/share/bareos/config/bareos-dir.d/catalog/MyCatalog.conf \
    usr/share/bareos/config/bareos-dir.d/client/bareos-fd.conf \
    usr/share/bareos/config/bareos-dir.d/console/bareos-mon.conf \
    usr/share/bareos/config/bareos-dir.d/director/bareos-dir.conf \
    usr/share/bareos/config/bareos-dir.d/fileset/Catalog.conf \
    usr/share/bareos/config/bareos-dir.d/fileset/LinuxAll.conf \
    usr/share/bareos/config/bareos-dir.d/fileset/SelfTest.conf \
    "usr/share/bareos/config/bareos-dir.d/fileset/Windows All Drives.conf" \
    usr/share/bareos/config/bareos-dir.d/job/backup-bareos-fd.conf \
    usr/share/bareos/config/bareos-dir.d/job/BackupCatalog.conf \
    usr/share/bareos/config/bareos-dir.d/job/RestoreFiles.conf \
    usr/share/bareos/config/bareos-dir.d/jobdefs/DefaultJob.conf \
    usr/share/bareos/config/bareos-dir.d/messages/Daemon.conf \
    usr/share/bareos/config/bareos-dir.d/messages/Standard.conf \
    usr/share/bareos/config/bareos-dir.d/pool/Differential.conf \
    usr/share/bareos/config/bareos-dir.d/pool/Full.conf \
    usr/share/bareos/config/bareos-dir.d/pool/Incremental.conf \
    usr/share/bareos/config/bareos-dir.d/pool/Scratch.conf \
    usr/share/bareos/config/bareos-dir.d/profile/operator.conf \
    usr/share/bareos/config/bareos-dir.d/schedule/WeeklyCycleAfterBackup.conf \
    usr/share/bareos/config/bareos-dir.d/schedule/WeeklyCycle.conf \
    usr/share/bareos/config/bareos-dir.d/storage/File.conf \
    usr/bin/bareos-dir \
    usr/share/man/man8/bareos-dir.8.gz \
    usr/share/man/man8/bareos.8.gz \
  ; do
    _cp "$srcdir/install/$f" "$pkgdir/$f"
  done

  _cp $srcdir/bareos/platforms/systemd/bareos-dir.service $pkgdir/usr/lib/systemd/system/bareos-dir.service
}

package_bareos-storage-fifo() {
  pkgdesc="${pkgdesc} - FIFO support for storage daemon"
  depends=("bareos-storage=$pkgver" 'zlib' 'libcap' 'openssl' 'jansson' 'lzo')
  for f in \
    usr/lib/bareos/backends/libbareossd-fifo-$_pkgver.so \
    usr/lib/bareos/backends/libbareossd-fifo.so \
    usr/share/bareos/config/bareos-dir.d/storage/NULL.conf.example \
    usr/share/bareos/config/bareos-sd.d/device/NULL.conf.example \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-tools() {
  pkgdesc="${pkgdesc} - CLI tools (bcopy, bextract, bls, bregeq, bwild)"
  depends=("bareos-common=$pkgver" 'glibc')

  for f in \
    usr/bin/bregex \
    usr/bin/bwild \
    usr/bin/bcopy \
    usr/bin/bextract \
    usr/bin/bls \
    usr/bin/bpluginfo \
    usr/share/man/man1/bwild.1.gz \
    usr/share/man/man1/bregex.1.gz \
    usr/share/man/man8/bcopy.8.gz \
    usr/share/man/man8/bextract.8.gz \
    usr/share/man/man8/bls.8.gz \
    usr/share/man/man8/bpluginfo.8.gz \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-filedaemon() {
  pkgdesc="${pkgdesc} - File daemon"
  depends=("bareos-common=$pkgver" 'jansson' 'zlib' 'gcc-libs')
  groups+=('bareos-client')
  install=bareos-filedaemon.install

  for f in \
    usr/share/bareos/config/bareos-fd.d/client/myself.conf \
    usr/share/bareos/config/bareos-fd.d/director/bareos-dir.conf \
    usr/share/bareos/config/bareos-fd.d/director/bareos-mon.conf \
    usr/share/bareos/config/bareos-fd.d/messages/Standard.conf \
    usr/bin/bareos-fd \
    usr/lib/bareos/plugins/bpipe-fd.so \
    usr/share/man/man8/bareos-fd.8.gz \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done

  _cp $srcdir/bareos/platforms/systemd/bareos-fd.service $pkgdir/usr/lib/systemd/system/bareos-fd.service
}

package_bareos-storage() {
  pkgdesc="${pkgdesc} - Storage daemon"
  depends=("bareos-common=$pkgver" 'zlib' 'lzo' 'jansson' 'openssl' 'libcap')
  install=bareos-storage.install

  for f in \
    usr/lib/bareos/plugins/autoxflate-sd.so \
    usr/lib/bareos/scripts/disk-changer \
    usr/share/bareos/config/bareos-sd.d/device/FileStorage.conf \
    usr/share/bareos/config/bareos-sd.d/director/bareos-dir.conf \
    usr/share/bareos/config/bareos-sd.d/director/bareos-mon.conf \
    usr/share/bareos/config/bareos-sd.d/messages/Standard.conf \
    usr/share/bareos/config/bareos-sd.d/storage/bareos-sd.conf \
    usr/bin/bareos-sd \
    usr/share/man/man8/bareos-sd.8.gz \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done

  install -d $pkgdir/var/lib/bareos/storage
  _cp $srcdir/bareos/platforms/systemd/bareos-sd.service $pkgdir/usr/lib/systemd/system/bareos-sd.service
}

package_bareos-storage-tape() {
  pkgdesc="${pkgdesc} - Tape support for storage daemon"
  depends=("bareos-storage=$pkgver" 'zlib' 'libcap' 'bash' 'openssl' 'jansson' 'lzo' "mtx")
  backup=(etc/bareos/mtx-changer.conf)

  for f in \
    etc/bareos/mtx-changer.conf \
    usr/share/bareos/config/bareos-dir.d/storage/Tape.conf.example \
    usr/share/bareos/config/bareos-sd.d/autochanger/autochanger-0.conf.example \
    usr/share/bareos/config/bareos-sd.d/device/tapedrive-0.conf.example \
    usr/lib/bareos/backends/libbareossd-gentape-$_pkgver.so \
    usr/lib/bareos/backends/libbareossd-gentape.so \
    usr/lib/bareos/backends/libbareossd-tape-$_pkgver.so \
    usr/lib/bareos/backends/libbareossd-tape.so \
    usr/lib/bareos/scripts/mtx-changer \
    usr/share/man/man8/bscrypto.8.gz \
    usr/share/man/man8/btape.8.gz \
    usr/bin/bscrypto \
    usr/bin/btape \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-director-python-plugin() {
  pkgdesc="${pkgdesc} - Python plugin for director daemon"
  depends=("bareos-director=$pkgver" 'libcap' 'jansson' 'lzo' 'python' 'python2')

  for f in \
    usr/lib/bareos/plugins/python-dir.so \
    usr/lib/bareos/plugins/bareos-dir.py.template \
    usr/lib/bareos/plugins/bareos_dir_consts.py \
    usr/lib/bareos/plugins/BareosDirWrapper.py \
    usr/lib/bareos/plugins/BareosDirPluginBaseclass.py \
    usr/lib/bareos/plugins/bareos-dir-class-plugin.py \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-filedaemon-python-plugin() {
  pkgdesc="${pkgdesc} - Python plugin for file daemon"
  depends=("bareos-filedaemon=$pkgver" 'python' 'python2' 'libcap' 'lzo' 'jansson')

  for f in \
    usr/lib/bareos/plugins/python-fd.so \
    usr/lib/bareos/plugins/bareos-fd.py.template \
    usr/lib/bareos/plugins/bareos-fd-local-fileset.py \
    usr/lib/bareos/plugins/bareos-fd-mock-test.py \
    usr/lib/bareos/plugins/BareosFdPluginBaseclass.py \
    usr/lib/bareos/plugins/BareosFdPluginLocalFileset.py \
    usr/lib/bareos/plugins/BareosFdWrapper.py \
    usr/lib/bareos/plugins/bareos_fd_consts.py \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}

package_bareos-storage-python-plugin() {
  pkgdesc="${pkgdesc} - Python plugin for storage daemon"
  depends=("bareos-storage=$pkgver" 'python2' 'lzo' 'python' 'libcap' 'jansson')

  for f in \
    usr/lib/bareos/plugins/python-sd.so \
    usr/lib/bareos/plugins/bareos-sd.py.template \
    usr/lib/bareos/plugins/bareos_sd_consts.py \
    usr/lib/bareos/plugins/BareosSdPluginBaseclass.py \
    usr/lib/bareos/plugins/BareosSdWrapper.py \
    usr/lib/bareos/plugins/bareos-sd-class-plugin.py \
  ; do
    _cp $srcdir/install/$f $pkgdir/$f
  done
}
