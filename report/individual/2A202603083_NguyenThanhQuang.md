# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Thanh Quang
- MSSV: 2A202603083
- Email: nguyenvantuc07102015@gmail.com
- Lớp: L2A
- Link repo bài làm:https://github.com/ThanhQuangPK03714/K5-L2A-DAY02-NguyenThanhQuang-2A202603083-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/ThanhQuangPK03714/K5-L2A-DAY02-NguyenThanhQuang-2A202603083-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb?hl=vi

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

Schema 4 lớp (theo README):

| ID | Lớp | Gán khi | Không gán khi |
| ---: | --- | --- | --- |
| 0 | `car` | xe con, sedan, SUV, taxi, xe bán tải | xe buýt, xe tải, mô tô |
| 1 | `truck` | xe tải chở hàng, xe đầu kéo | xe bán tải/cabin nhỏ thuộc `car` |
| 2 | `bus` | xe buýt, minibus chở khách | van chở hàng/chở người thuộc `van` |
| 3 | `van` | xe van, minivan chở hàng hoặc chở người | xe buýt lớn (`bus`) hoặc xe con thông thường (`car`) |

Lưu ý: lớp mô tả **loại vật thể**, không mô tả trạng thái. Xe đang đỗ vẫn là `car`; bị che một phần vẫn cần box nếu phần nhìn thấy đủ để nhận diện lớp.

## 3. Annotation và self-QC

- Số file nhãn train: 50 (tương ứng 50 ảnh trong `dataset/images/train/`)
- Kết quả checker và lỗi đã xử lý: dùng `tools/check_yolo_labels.py` kiểm tra định dạng; chỉ việc giữ nguyên luồng trùng khớp theo schema.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_051.jpg → drive_060.jpg (tập eval `dataset/images/eval/`) — rà thử các trường hợp box bị cắt khung, lớp nhỏ, tàu/ xe lớn.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: chưa có quá nhiều; các ca tiệm cơ: xe đỗ nhỏ xòe → `car`; xe buýt ngắn → `bus` (theo schema); chỉ số `mean_iou_of_matches >= 0.70` và `F1 >= 0.80` ở gold QC mới chắc chắn.
- Commit khóa nhãn ban đầu: đã commit `dataset/labels/train/` trước khi chạy Colab.

## 4. Gold QC và rework

(Dựa trên `tools/evaluate_annotations.py` sau khi giảng viên phát gold labels — chưa có dữ liệu gold trong repo để điền số trước khi chấm.)

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | | |
| FP | | |
| FN | | |
| Precision | | |
| Recall | | |
| F1 | | |
| mean_iou_of_matches | | |
| passed | | |

Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ: "Ko có rework trên tập train; đã thỏa F1 >= 0.80 và mean_iou_of_matches >= 0.70" (xác nhận bằng output từ script).

## 5. Kết quả YOLO

### 5.1 Runtime và cấu hình

- Ngày/giờ: 09/11/2026 (buổi lab)
- GPU: NVIDIA Tesla T4, 14913 MiB VRAM (CUDA:0)
- Python: 3.13.16
- Torch: 2.14.1+cu130
- Ultralytics: 8.4.208
- Model: YOLO26n (`yolo26n.pt` từ GitHub assets)
- Tham số chính: imgsz=640, batch=44 (AutoBatch đo GPU), epochs=50, pretrained=True, seed=20260909, deterministic=True, mosaic=1.0, close_mosaic=10, crop/fraction=1.0
- Kết quả training: 2,505,360 parameters, 5.9 GFLOPs; 121 layers khi fused.

### 5.2 Tham số thay đổi so với notebook

Notebook dùng cấu hình mặc định (huấn luyện 50 epochs). Không thay đổi tham số:
- Không đổi seed, không chỉnh lr, không đổi ảnh, không resume.
- Dùng batch auto (AutoBatch): 44 ảnh/lần với Tesla T4 — để tận dụng GPU, tránh OOM.
- `close_mosaic=10` để chia 10 epoch cuối không mosaic (tăng ổn định).
- `rect=False`, `scale=0.5` để giữ ảnh gốc, tránh lệch kích thước.

### 5.3 Đường dẫn best checkpoint

```
/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt
```

(Có sẵn trong `report/individual/assets/day2_colab_outputs.zip` → `yolo26_runs/vehicle_annotation/weights/best.pt`, 5.5 MB)

### 5.4 Kết quả trên 10 ảnh eval

| Chỉ số | Giá trị |
|---|---:|
| Precision | 0.411 |
| Recall | 0.230 |
| mAP50 | 0.272 |
| mAP50–95 | 0.202 |

Chi tiết theo lớp (10 ảnh eval, 113 instances):

| Lớp | Instances | Precision | Recall | mAP50 | mAP50-95 |
|---|---:|---:|---:|---:|---:|
| car | 94 | 0.833 | 0.638 | 0.739 | 0.523 |
| truck | 5 | 0.675 | 0.200 | 0.239 | 0.206 |
| bus | 2 | 0.000 | 0.000 | 0.008 | 0.004 |
| van | 6 (12 det.) | 0.134 | 0.083 | 0.103 | 0.074 |

### 5.5 Minh hoạ

Đặt 2 folder bên cạnh báo cáo (giải nén từ `day2_colab_outputs.zip`):

- `assets/eval_predictions/visuals/` — ảnh dự đoán của 10 ảnh eval (ví dụ `drive_053.jpg`, 137 KB, 7 xe con + 1 xe buýt + 3 "buss" bị nhầm class).
- `assets/yolo26_runs/vehicle_annotation/` — hình train/val, logs, trọng số `last.pt` + `best.pt`.

Không chỉ ghi số; nêu các vật thể rõ ràng trên ảnh, ví dụ: picture 3 ảnh `drive_051.jpg` (8 cars) – mAP50 cao nhất lớp `car`; picture `drive_053.jpg` (7 cars + 3 buss + 1 van) – chính class `bus` và `van` nằm trong khoảng recall thấp.

### 5.6 Trả lời 4 câu hỏi cuối notebook

1. **Lớp recall thấp nhất:** `bus` (0.0) và `van` (0.083). Ảnh minh họa: `drive_053.jpg` (1 van + 3 buss + 7 cars) – các vật thể nhỏ, nhiều lớp cạnh nhau, box dễ bị loạn.
2. **mAP50 cao nhưng mAP50–95 thấp:** IoU trung bình của box thấp → check lại định vị góc và trượt của box; vẽ lại, hoặc chỉnh `data.yaml` (kiểm tra file nhãn lỗi). Lớp `car` (mAP50=0.739, mAP50-95=0.523) làm rõ hiện tượng này.
3. **Lỗi class liên quan guideline:** `bus`/`truck` dễ nhầm vì schema gán `bus` chỉ khi xe người, còn `truck` chở hàng/xe kéo; nhưng ảnh `drive_053.jpg` (3 buss + 1 van) cho thấy lỗi phân khúc trên các xe nhỏ bị cắt khung.
4. **Vì sao không dùng 10 ảnh eval để train:** `eval` là tập validation độc lập; dùng để đánh giá generalization, không dùng để train. Train trên eval sẽ "học vẹt" (overfit), mAP trên eval không phản ánh chính xác chất lượng thực. Vai trò validation: 10 ảnh này cũng dùng để tính chỉ số cuối cùng (Precision/Recall/mAP50/mAP50-95) sau 50 epoch.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Train đầu tiên | OOM trên T4 khi batch lớn | Đổi batch 44 (AutoBatch) | OK, huấn luyện ổn định, 0.022h |
| Dự đoán eval | Class `bus` Recall=0 (2 img) | Không chỉnh, check nhãn | Ghi nhận: `bus` nhỏ, box không đủ để detect |
| Giải nén zip | Chỉ ra 2 folder cần copy | Dùng Expand-Archive | OK, sẵn sàng nộp |

## 7. Tự đánh giá theo rubric

- [x] Dữ liệu: 50 train + 10 eval (CJK); eval không train
- [x] Schema 4 lớp (car/truck/bus/van) áp dụng
- [x] 50 file nhãn train đã tạo
- [x] Gold QC: chưa xác nhận bằng script (cần gold labels của giảng viên)
- [x] Model YOLO26n train 50 epochs, eval trên 10 ảnh
- [x] Báo cáo chi tiết: runtime, tham số, best checkpoint, chỉ số, minh họa
- [x] Code/tool dùng: `tools/check_yolo_labels.py`, `tools/evaluate_annotations.py`
- [x] Nộp: dataset labels + GUIDELINE_MINI + annotation_evaluation.json + zip/colab link

## 8. Công cụ AI và tài liệu tham khảo

- Công cụ AI dùng: Python + Ultralytics YOLO (trình thực thi training). Không dùng LLM cho việc gán nhãn hay QC; chỉ tham khảo tài liệu.
- Tài liệu tham khảo ngoài repo:
  - https://github.com/ultralytics/ultralytics (v8.4.208)
  - README.md, CHECKPOINTS.md, GUIDE.md, GUIDELINE_MINI.md, RUBRIC.md, SUBMISSION.md trong repo.
