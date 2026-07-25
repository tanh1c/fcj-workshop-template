---
title: "Dataset bước 4: Kiểm tra labels và splits"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

Chạy local preprocessing preview miễn phí trước khi dùng SageMaker:

```bash
python preprocessing/processing_script.py \
  --input data_generation/combined_trajectories.jsonl \
  --output-dir data/processed \
  --seed 42
```

Xác nhận:

- Có đủ sáu labels mong đợi.
- Mỗi row có 17 ordered features và `label`.
- Các source indicators loại trừ nhau.
- Safety booleans trở thành số sau feature extraction.
- Fixed-seed split tạo train, validation và held-out test CSV.
- Không đưa label vào inference hoặc Model Monitor feature input.

Tỷ lệ split là 70% train, 15% validation và 15% test. Giữ test set để held-out evaluation; không tune theo tập này.

Trước mọi production claim, phải thay hoặc bổ sung synthetic records bằng trajectory thực đại diện và human labels được review độc lập.
