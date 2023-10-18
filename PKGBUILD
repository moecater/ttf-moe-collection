pkgbase=ttf-moe-collection
#pkgname=($pkgbase ttf-{windows,p10k-nf} ttf-lxgw-{wenkai{,-mono,-screen},neoxihei})
pkgname=($pkgbase ttf-lxgw-neoxihei-screen-full)
pkgver=r6
pkgrel=1
license=('custom')
pkgdesc='A font collection made by moe and aims to provide CJK users a better experience'
url='https://github.com/moecater/PKGBUILD/tree/main/ttf-moe-collection'
arch=('any')
depends=('ttf-sarasa-gothic' 'ttf-firacode-nerd' 'noto-fonts' 'noto-fonts-emoji')
makedepends=(
  'git'
  #'unzip'
  # 'p7zip'
  # font-patcher.sh
  #'python-pdm'
  #'fontforge'
)
source=(
  'file://moe-fonts-preference.conf'
  'file://dracula-tty.sh'
  # windows
  # put your ttf/ttc file extract from windows DIRECTLY without wraping by any folder in a zip archive named `WindowsFonts.zip` and put it the same folder as `PKGBUILD`
  # make use of msys_extreact_fonts.sh
  #'file://WindowsFonts.zip' 
  
  # nerd-font patcher
  #'font-patcher::https://raw.githubusercontent.com/ryanoasis/nerd-fonts/master/font-patcher'
  
  # p10k-nf
  #'MesloLGS-NF-Regular.ttf::https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf'
  #'MesloLGS-NF-Bold.ttf::https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf'
  #'MesloLGS-NF-Italic.ttf::https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf'
  #'MesloLGS-NF-Bold-Italic.ttf::https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf'
  # lxgw-wenkai
  #'https://github.com/lxgw/LxgwWenKai/raw/main/fonts/TTF/LXGWWenKai-Bold.ttf'
  #'https://github.com/lxgw/LxgwWenKai/raw/main/fonts/TTF/LXGWWenKai-Light.ttf'
  #'https://github.com/lxgw/LxgwWenKai/raw/main/fonts/TTF/LXGWWenKai-Regular.ttf'
  
  # lxgw-wenkai-mono
  #'https://github.com/lxgw/LxgwWenKai/raw/main/fonts/TTF/LXGWWenKaiMono-Bold.ttf'
  #'https://github.com/lxgw/LxgwWenKai/raw/main/fonts/TTF/LXGWWenKaiMono-Light.ttf'
  #'https://github.com/lxgw/LxgwWenKai/raw/main/fonts/TTF/LXGWWenKaiMono-Regular.ttf'
  
  # lxgw-wenkai-screen
  #'https://github.com/lxgw/LxgwWenKai-Screen/releases/latest/download/LXGWWenKaiScreen.ttf'
  #'https://github.com/lxgw/LxgwWenKai-Screen/releases/latest/download/LXGWWenKaiScreenR.ttf'
  
  # lxgw-neoxihei-screen-full
  'https://github.com/lxgw/LxgwNeoXiHei-Screen/releases/download/v1.105/LXGWNeoXiHeiScreenFull.ttf'
)
noextract=(
  #'WindowsFonts.zip'
)
sha256sums=('e2558f47eb351bfecb65a5ea27eb2ffdd1a1f16bc94fa87df12fd17614872e01'
            'ef56fa67d8fbdbf14e10f46d09396c289867e790d73b2cb50755efc155f87369'
            '50ac3ca5209c0ec2532d239582abcba5ea6637935d1d32ea00b74c056f5c3742')

pkgver() {
  printf "r%s" "$(git rev-list --count HEAD)"
}

prepare() {
  #pdm init -n
  #pdm add configparser
  #git -C nerd-fonts pull || git clone --sparse --depth 1 --branch master --single-branch --filter=blob:none https://github.com/ryanoasis/nerd-fonts.git nerd-fonts
  #[[ -d src/glyphs ]] && return 0
  #git -C nerd-fonts sparse-checkout set src/glyphs
  #ln -srf nerd-fonts/src .
  echo 0
}

_prepare() {
  rm -rf "$pkgname"
  mkdir -p "$pkgname"
  rm -rf "${pkgname}"_new
  mkdir -p "${pkgname}"_new
}

_build() {
  local fonts=("$@")
  for i in "${fonts[@]}"; do
    pdm run fontforge -script font-patcher -out "${pkgname}_new" "$i"
  done
}

_pass_build() {
  local fonts=("$@")
  for i in "${fonts[@]}"; do
    mv "$i" "${pkgname}"_new/
  done
}

_package() {
  local fonts
  mapfile -t fonts < <(ls -d "${pkgname}"_new/*.ttf)
  mapfile -O ${#fonts[@]} -t fonts < <(ls -d "${pkgname}"_new/*.ttc)
  for i in "${fonts[@]}"; do
    install -Dm644 "$i" -t "$pkgdir/usr/share/fonts/TTF"
  done
}

package_ttf-windows() {
  [[ ! -f "WindowsFonts.zip" ]] && return 0
  _prepare
  unzip -q -o 'WindowsFonts.zip' -d "$pkgname"
  ## If you prefer to patch it as nerd-font, make use of codes below
  # buildfonts=(
    # msgothic.ttc            # MS Gothic
    # YuGothR.ttc YuGothB.ttc # Yu Gothic
    # YuGothM.ttc             # Yu Gothic Medium
    # YuGothL.ttc             # Yu Gothic Light
    # malgun.ttf malgunbd.ttf # Malgun Gothic
    # malgunsl.ttf            # Malgun Gothic Semilight
    # simsun.ttc              # NSimSun
    # simsunb.ttf             # SimSun-ExtB
    # msyh.ttc msyhbd.ttc     # Microsoft YaHei
    # msyhl.ttc               # Microsoft YaHei Light
    # msjh.ttc msjhbd.ttc     # Microsoft JhengHei
    # msjhl.ttc               # Microsoft JhengHei Light
    # mingliub.ttc            # MingLiU_HKSCS-ExtB
  # )
  # local fonts
  # for i in "${buildfonts[@]}"; do
  #   fonts+=("${pkgname}/${i}")
  # done
  # _build "${fonts[@]}"
  # unset fonts

  local fonts
  mapfile -t fonts < <(ls -d "$pkgname"/*.ttf)
  mapfile -O ${#fonts[@]} -t fonts < <(ls -d "$pkgname"/*.ttc)
  _pass_build "${fonts[@]}"
  _package
}

# package_ttf-firacode-nf() {
#   _prepare
#   unzip -q -o 'FiraCodeNF.zip' -d "$pkgname"
#   local fonts
#   mapfile -t fonts < <(ls -d "$pkgname"/*Mono.ttf)
#   mapfile -O ${#fonts[@]} -t fonts < <(ls -d "$pkgname"/*Complete.ttf)
#   _pass_build "${fonts[@]}"
#   _package
#   install -Dm644 "$pkgname/LICENSE" -t "$pkgdir/usr/share/licenses/$pkgname"
# }

package_ttf-p10k-nf() {
  _prepare
  local fonts=(
    'MesloLGS-NF-Regular.ttf'
    'MesloLGS-NF-Bold.ttf'
    'MesloLGS-NF-Italic.ttf'
    'MesloLGS-NF-Bold-Italic.ttf'
  )
  _pass_build "${fonts[@]}"
  _package
}

package_ttf-lxgw-wenkai() {
  _prepare
  local fonts=(
    'LXGWWenKai-Bold.ttf'
    'LXGWWenKai-Light.ttf'
    'LXGWWenKai-Regular.ttf'
  )
  _pass_build "${fonts[@]}"
  _package
}

package_ttf-lxgw-wenkai-mono() {
  _prepare
  local fonts=(
    'LXGWWenKaiMono-Bold.ttf'
    'LXGWWenKaiMono-Light.ttf'
    'LXGWWenKaiMono-Regular.ttf'
  )
  _pass_build "${fonts[@]}"
  _package
}

package_ttf-lxgw-wenkai-screen() {
  _prepare
  local fonts=('LXGWWenKaiScreen.ttf' 'LXGWWenKaiScreenR.ttf')
  _pass_build "${fonts[@]}"
  _package
}

package_ttf-lxgw-neoxihei-screen-full() {
  _prepare
  local fonts=('LXGWNeoXiHeiScreenFull.ttf')
  _pass_build "${fonts[@]}"
  _package
}

package_ttf-moe-collection() {
  cd "${pkgsrc}"
  install -Dm644 "moe-fonts-preference.conf" -T "${pkgdir}/usr/share/fontconfig/conf.default/99-moe-fonts-preference.conf"
  install -Dm755 "dracula-tty.sh" -t "${pkgdir}/etc/profile.d/"
}
