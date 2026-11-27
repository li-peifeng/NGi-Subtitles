# NGi-Subtitles

![字幕总数 4,088 · 更新 2026-11-27](https://img.shields.io/badge/%E5%AD%97%E5%B9%95%E6%80%BB%E6%95%B0%204%2C088-%E6%9B%B4%E6%96%B0%202026--11--27-blue) <!-- subtitle-pool-badge -->

## 强烈感谢 jzdxjk 大佬的制作整理

MDC-NGi 字幕池。下载字幕请进入 [Subtitles](Subtitles)，程序可直接读取根目录的 [tree.json](tree.json)。

## 上传字幕

1. `jzdxjk` 可以在自己的 Fork 中，从本仓库最新的 `main` 新建分支。
2. 可以直接在 [Subtitles](Subtitles) 中新增或替换命名正确的字幕，也可以将字幕打包成 ZIP 上传到 [update](update)。一次可以提交多个文件。
3. 提交 Pull Request 到本仓库的 `main`。GitHub Actions 校验通过后会自动合并，并刷新 `tree.json`。PR 中禁止删除或移动已有文件，也禁止手动修改索引、脚本和工作流。

字幕文件名使用 `番号前缀-数字.后缀`，分集字幕可加上 `-CD1`、`-CD2` 等分集标记。ZIP 可以包含子目录，入库时会按文件名归类。前缀和分集标记转为大写，后缀转为小写，数字和前导零保留。无关文件会被跳过，每个 ZIP 至少要包含一个符合命名格式的字幕。

- `abcdefg-12345.SRT` → `Subtitles/A/ABCDEFG/ABCDEFG-12345.srt`
- `ABCDEFG-12345.ass` → `Subtitles/A/ABCDEFG/ABCDEFG-12345.ass`
- `1abc-00001.srt` → `Subtitles/1/1ABC/1ABC-00001.srt`
- `fc2-4990459-cd2.SRT` → `Subtitles/F/FC2/FC2-4990459-CD2.srt`

同一番号的各分集和不同后缀可共存。上传的字幕与已有文件同路径时，新内容会覆盖旧内容。一次 PR 中，同一路径只能提交一种内容。单个 ZIP 最大 50 MiB，解压后合计最大 128 MiB、最多 10,000 项，单个字幕最大 16 MiB。

成功处理的 ZIP 会移出 `update/`，原提交仍可在 Git 历史中查看。管理员直接上传到 `main` 也会触发入库。此时如果有 ZIP 处理失败，Actions 日志会显示原因，失败的 ZIP 会留在 `update/`，同批校验通过的 ZIP 照常入库。

## 字幕索引

`tree.json` 按首字符、番号前缀、字幕文件名分组。例如：

```json
{
  "A": {
    "ABCDEFG": [
      "ABCDEFG-12345.ass",
      "ABCDEFG-12345.srt"
    ]
  }
}
```

读取时按 `Subtitles/{首字符}/{番号前缀}/{文件名}` 拼接路径。每次入库只更新有变动的分组。同路径覆盖时，索引中的文件名保持一致。管理员可在 [Actions](https://github.com/li-peifeng/NGi-Subtitles/actions/workflows/subtitle-pool.yml) 手动运行字幕入库。勾选「从 Subtitles 全量重建索引」可按当前目录重新生成索引。
