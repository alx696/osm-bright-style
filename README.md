# OSM Bright

源自 [OSM Bright](https://github.com/openmaptiles/osm-bright-gl-style/) , 针对 [里路路书](lilu.red) 需要进行了调整.

## 声明

政治是个很麻烦的东西, 我们希望规避这些风险. 此样式是纯粹的路书用途, 不提供任何国界水域边界容易触及政治的信息.

## 开发

### 生成sprite

```
sudo npm install --force -g @beyondtracks/spritezero-cli

# 在 icons 文件夹同级位置执行, icons 可从 https://github.com/openmaptiles/osm-bright-gl-style 下载

spritezero osm-bright icons/

spritezero --retina osm-bright@2x icons/
```

> 注意: mapbox的spritezero-cli无法安装. Android中需要使用 `@2x` 高清版本.

### 生成glyphs

```
sudo npm install -g fontnik

build-glyphs /path/to/NotoSerifSC-Bold.otf /path/to/Noto Serif SC Bold
```

> 示例使用的是 `NotoSerifSC-Bold.otf` , 你可以替换成其它的字体.

glyphs 配置 ` "glyphs": "asset://glyphs/{fontstack}/{range}.pbf"` 中的 `{fontstack}` 需要与上面生成的文件夹名称一致, 也要与 style.json 中的 `text-font` 一致.

比如 build-glyphs 执行后输出到了名为 `nssb` 的文件夹中, 那把整个文件夹放到 `asset://glyphs` 中, 然后style.json 中的 `text-font` 设为 `["nssb"]` 即可应该该字体.

### 样式

了解 [规范](https://openmaptiles.org/docs/style/mapbox-gl-style-spec/) , 根据 sprite 和 glyphs 资源, 结合矢量切片数据, 最终编写出可用的样式文件, 即 style.json .

使用 [Planetiler](https://github.com/onthegomap/planetiler/) 获取矢量切片数据的 mbtiles(sqlite) 文件, 通过 [TileServer GL](https://openmaptiles.org/docs/host/tileserver-gl/) 启动地图服务, [参考样式概要](https://openmaptiles.org/docs/style/mapbox-gl-style-spec/) 使用 [maputnik](https://github.com/maputnik/editor) 本地调试样式.

---

## 注意

[在线样式](style-localhost.json) 与 [Android离线样式](style-android-offline.json) 的区别在 `sources` , `sprite` , `glyphs` 上, 其它部分一致.

离线源配置:

```
    "sources": {
        "openmaptiles": {
            "type": "vector",
            "tiles": [
                "mbtiles://{PATH_TO_MBTILES}"
            ],
            "maxzoom": 14
        }
    }
```

> 使用 style.json 前把 `{PATH_TO_MBTILES}` 替换成 mbtiles 文件的绝对路径. `maxzoom` 必须设置成与 mbtiles 文件中 metadata 表的 maxzoom 值一致, 否则地图缩放级别超过此值时所有地图标记都会消失(通过 TileServer GL 在线显示时不设置maxzoom也没问题, 但离线使用mbtiles 文件时则必须设置)!
