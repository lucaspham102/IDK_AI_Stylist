export interface ActiveOutfitState {
// Bối cảnh đã chọn
context: ContextState;

// Cấu hình áo chính
garment: {
type: 'ngu\_than' | 'ao\_dai' | 'tu\_than' | 'nhat\_binh';
sleeve: 'tay\_chen' | 'tay\_thung'; // Tay chẽn (dạo phố) hoặc Tay thụng (lễ phục)
colorId: string;
isFlapReversed: boolean; // false = Vạt Tả đè Hữu (Chuẩn) | true = Vạt Hữu đè Tả (Lỗi Đỏ 🔴)
};

// Danh sách ID phụ kiện đang mặc (null nếu không chọn)
accessories: {
head: string | null;     // Mũ, mấn, kính râm
foot: string | null;     // Hài, guốc mộc, sneaker
bag: string | null;      // Giỏ mây, túi baguette

waitst : string |null 	// ID dây đai / dải ngọc 

waistPendant : String |null ;     // ID xà tích bạc / ngọc bội treo eo
pattern: string | null;  // Họa tiết thêu
};

// Dữ liệu khuôn mặt người dùng (Ghép tại S2)
userFace: {
isCustom: boolean;       // Người dùng có tải ảnh lên không
imageUrl: string | null; // Data URL ảnh chân dung
scale: number;           // Mức phóng to/thu nhỏ (0.5 - 2.0)
offsetX: number;         // Tọa độ dịch chuyển ngang (px)
offsetY: number;         // Tọa độ dịch chuyển dọc (px)
};
}

