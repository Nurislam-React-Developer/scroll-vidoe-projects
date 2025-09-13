# Apple-style Scroll Video (Optimized)

- Canvas фиксирован на 100vh, секция — 220vh для скролла.
- GSAP + ScrollTrigger управляет покадровой анимацией.
- WebP кадры (качество ~80), манифест `public/animation/manifest.json`.
- Прогресс-бар и ленивый догруз кадров.

## Генерация кадров через ffmpeg (WebP)

```bash
# 1) Создайте папку
mkdir -p public/animation

# 2) Экспорт кадров WebP из видео (пример на ~900 кадров)
ffmpeg -i video.mp4 -vf scale=1440:-1 -q:v 80 public/animation/%04d.webp
# Для точного контроля количества кадров:
# -r 30  (ставит частоту кадров), -ss/-to для обрезки и пр.
# Пример: 30 fps на 30 секунд даст ~900 кадров

# 3) Сформируйте manifest.json (список файлов)
python - <<'PY'
import os, json
files = sorted([f"public/animation/{f}" for f in os.listdir("public/animation") if f.endswith(".webp")])
with open("public/animation/manifest.json","w",encoding="utf-8") as w:
    json.dump({"frames": files, "count": len(files), "format": "webp"}, w, ensure_ascii=False, indent=2)
PY
```

## В этой сборке
Извлечено кадров: **200** из `/mnt/data/1175-143563015_small.mp4`.
