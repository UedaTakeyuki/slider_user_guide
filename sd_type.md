# slider SD カードの種類

Slider の SD カードは用途に合わせて２種類用意しています

- gc15: デスクトップを持つタイプ
- gc16: デスクトップを持たないタイプ

## gc15 デスクトップタイプ

gc15 は Raspbian Jessie をベースに slider の機能を構築した SD カードで、Windows のように **デスクトップを利用することが可能** です

<img src="pic/ss.2016-12-16 14.44.26.png" width="50%">

ただし、デスクトップアイコン等のリソースに EXT4 の領域が追加で必要になる分、次の gc16 よりも拡張boot領域が小さくなります

## gc16 コンソールタイプ

gc16 は Raspbian Jessie lite をベースに slider の機能を構築した SD カードで、Windows のようなデスクトップは持たず、**コンソールからの利用のみが可能** です

<img src="pic/ss.2016-12-16 14.44.14.png" width="50%">

gc15 より拡張 boot 領域が大きくなります
