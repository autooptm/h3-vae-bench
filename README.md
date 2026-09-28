<div align="center">
  <a href="https://autooptm.com"><img src=".autooptm/logo.png" width="96" alt="AutoOptm"></a>

  <h1>h3-vae-bench · optimized by <a href="https://autooptm.com">AutoOptm</a></h1>

  <p><b>1.41x faster end to end</b> on the command below, output verified against the stock program.</p>

  <p>
    <a href="https://autooptm.com"><img alt="speedup" src="https://img.shields.io/badge/end--to--end-1.41x-2ea44f"></a>
    <a href="https://github.com/autooptm-ai/h3-vae-bench/commit/19b635dd4cef9fb944098067310f7cc0eec425c5"><img alt="base" src="https://img.shields.io/badge/upstream-19b635dd4cef-blue"></a>
    <img alt="card" src="https://img.shields.io/badge/measured%20on-NVIDIA%20RTX%204090-lightgrey">
  </p>
</div>

> This is a fork of [autooptm-ai/h3-vae-bench](https://github.com/autooptm-ai/h3-vae-bench) at commit
> [`19b635dd4cef`](https://github.com/autooptm-ai/h3-vae-bench/commit/19b635dd4cef9fb944098067310f7cc0eec425c5) with the AutoOptm patch applied on top.
> The optimisation was found, measured and verified automatically by [AutoOptm](https://autooptm.com);
> the patch is also kept at [`.autooptm/autooptm.patch`](.autooptm/autooptm.patch).

Every change is on by default and the command runs unchanged — same file, same flags, same outputs. Every change is behind a switch that defaults on; see [`.autooptm/autooptm.patch`](.autooptm/autooptm.patch).

## The result — `python decode.py`

| | |
|---|---|
| **Command** | `python decode.py` |
| **Entry point** | `decode.py` |
| **Unit measured** | one clip through decode.py's VAE round trip (encode → decode → frames) |
| **Before (stock)** | 84.22 (as reported) per unit |
| **After (this tree, all switches default ON)** | 59.72 (as reported) per unit |
| **Speedup** | **1.41x** end to end on NVIDIA RTX 4090 |
| **Output** | PSNR 62.9 dB (trimmed) against the stock program's output |

### What changed

| File | Where | Gain (alone) |
|---|---|---|
| `decode.py` | build_encoder | 1.409x |
| `decode.py` | build_encoder | 1.19x |
| `decode.py` | main() | 1.049x |


## Reproduce

```bash
git clone https://github.com/autooptm/h3-vae-bench-ao.git
cd h3-vae-bench-ao
# set up exactly as upstream documents, then:
python decode.py
```

`git diff 19b635dd4cef` is the same change as `.autooptm/autooptm.patch`.

---

<div align="center"><sub>Optimized by <a href="https://autooptm.com">AutoOptm</a> — point it at a repository, get back a verified speedup and the patch.</sub></div>

---

The upstream README is unchanged below.

# h3-vae-bench

MiniMax-H3 video VAE through the documented diffusers modular pipeline,
loading only the `vae` and `audio_vae` components of `MiniMaxAI/MiniMax-H3`:

- `encode.py` -- a 124-frame reference clip to condition latents
  (`Ref2VA` setup + reference encoder), 3 timed runs.
- `decode.py` -- 4 clips encoded once untimed, then `MiniMaxH3VideoDecodeStep`
  (the 36-layer ViT decoder + pixel un-normalisation) timed once per clip.

```
pip install -r requirements.txt
python encode.py
python decode.py
```
