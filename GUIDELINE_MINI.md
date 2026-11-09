# Mini guideline — bài làm cá nhân

> Nội dung sẽ điền trước khi bắt đầu gán nhãn. Xem lại và lưu ý lại các quyết định sau khi đã làm xong self-QC (check 10+ ảnh, benchmark ngẫu nhiên).

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ / điểm khác biệt |
| --- | --- | --- |
| `car` | Xe con: sedan, SUV, taxi, xe bán tải | Xe buýt, mô tô, van không thuộc lớp này |
| `truck` | Xe tải chở hàng, xe đầu kéo | Xe con, xe buýt không thuộc lớp này |
| `bus` | Xe buýt, minibus chở khách | Xe van, xe con không thuộc lớp này |
| `van` | Xe van, minivan chở hàng hoặc người | Xe buýt lớn, xe con thường không thuộc |

## Quy tắc chung — box

- **Bám sát** vật thể có nghĩa: box sát phần thân xe mình nhìn thấy, không lấy thêm đường viền, đèn, xe bên cạnh.
- **Không kéo quá** biên ảnh: mảnh bên ngoài bị cắt khung hoặc mờ ở biên chỉ box phần rõ ràng.
- **Không tự đoán** khi ảnh quá mờ, tâm cảnh không rõ, hoặc vật thể vượt biên: ghi câu lý do và hỏi giáo viên.
- **Từng xe một box riêng**: nếu 2 xe sát nhau, chạm vào nhau thì cứ vạch 2 box riêng một trên phần nhìn thấy, không gộp chung.

## Ca mơ hồ — ít nhất 3 trường hợp đặc biệt

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Xe đỗ yên bên đường, mảnh sau bị cắt khung | Gán lớp theo kiểu xe, chỉ box phần chiếc đầu nhìn thấy, không đoán phần ngoài | Mọi vật thể rõ ràng đều thuộc schema, box không vượt biên ảnh |
| `drive_017` | Mô tô đậu cạnh làn, hẹp lại chắn 1 phần | Không gán mô tô; chỉ box xe còn lại | Mô tô không thuộc schema (chỉ `car/truck/bus/van`) |
| `drive_026` | Bảng cầu hiển thị hình xe, phần dưới lại là biển báo | Không gán. Ghi `drive_026` và hỏi giáo viên | Cảnh hậu/hình biển báo không phải vật thể thực trong schema |
| `drive_034` | Xe van chở hàng, mép bên chật chồng vào xe khác | Gán `van`, box riêng cho mỗi xe phần nhìn thấy, ghi yêu cầu xác nhận | Van chở hàng thuộc schema; phần chồng xét riêng + hỏi giáo viên |

> Lưu ý cuối: sau khi làm xong, tự đọc lại các quyết định trước khi nộp; nếu gold chỉ ra sai thì sửa và ghi lại trong báo cáo.
