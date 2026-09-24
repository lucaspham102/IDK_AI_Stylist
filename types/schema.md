# 📐 DATA_CONTRACT_AND_CANVAS_SPEC.md
> **Dự án:** Việt Phục Remix  
> **Mục đích:** Quy chuẩn kích thước Canvas đồ họa và Hợp đồng dữ liệu duy nhất (Single Source of Truth) giữa Frontend và Backend/Engine[cite: 1].

---

## PHẦN I: QUY CHUẨN KÍCH THƯỚC CANVAS & KỶ LUẬT ASSET

### 1. Kích thước khung vẽ (Standard Dimensions)
* **Khung Canvas phòng thử đồ (S4 Preview):** Cố định chuẩn **`600px × 800px`** (Tỉ lệ 3:4)[cite: 1].
* **Khung xuất ảnh Lookbook (S5 Export):** Chuẩn **`1080px × 1920px`** (Tỉ lệ 9:16 dạng Story di động)[cite: 1].
* **Căn chỉnh hiển thị:** Canvas preview hiển thị responsive qua CSS: `aspect-[3/4] w-full max-w-[450px] relative overflow-hidden`.

### 2. Kỷ luật xuất file ảnh PNG (Strict Asset Rules for Member B)
* **Kích thước Artboard bắt buộc:** Mọi tệp ảnh phụ kiện/trang phục xuất ra phải nằm trọn trên cùng một khung ảnh **đúng `600px × 800px`**, định dạng PNG trong suốt (transparent background)[cite: 1].
* **Quy tắc "Zero Tight-Crop":** Tuyệt đối **không** xén sát mép chi tiết[cite: 1]. Ví dụ: Kính mát hay Đôi hài dù có diện tích nhỏ vẫn phải giữ nguyên khung viền `600x800px` bao quanh và nằm đúng vị trí tọa độ mắt/chân của ma-nơ-canh gốc[cite: 1].
* **Tối ưu dung lượng:** Toàn bộ file PNG sau khi vẽ phải nén qua công cụ như TinyPNG (mục tiêu mỗi file < 150KB) để tránh giật lag khi tải trên trình duyệt.

### 3. Cấu trúc xếp tầng hiển thị (CSS Layering & Z-Index)
Các lớp ảnh được bố trí xếp đè theo thứ tự `z-index` từ dưới lên trên, áp dụng thuộc tính `absolute inset-0 w-full h-full pointer-events-none`[cite: 1]:

[Layer 9] z-index: 90 ── Mũ / Khăn vấn / Kính mát (head)
[Layer 8] z-index: 80 ── Túi xách cầm tay (bag)
[Layer 7] z-index: 70 ── Vật treo eo: Ngọc bội / Xà tích (waistPendant)
[Layer 6] z-index: 60 ── Dải đới / Thắt lưng (belt)
[Layer 5] z-index: 50 ── Áo ngoài: Ngũ Thân / Dài / Tứ Thân (garment)
[Layer 4] z-index: 40 ── Giày / Hài / Guốc / Sneaker (foot)
[Layer 3] z-index: 30 ── Quần lụa / Thường xếp ly / Váy đụp (bottom)
[Layer 2] z-index: 20 ── Mặt người dùng cắt từ ảnh Oval (userFace)
[Layer 1] z-index: 10 ── Phôi Avatar gốc (Base Body)
[Layer 0] z-index: 00 ── Phông nền bối cảnh (Background Scene)

*(Ngoại lệ: Layer 5 của Áo ngoài tách thành 2 tệp vạt áo: Vạt Tả và Vạt Hữu. Khi gạt nút đảo vạt, hoán đổi `z-index` giữa 2 vạt này thay vì lật gương cả nhân vật)[cite: 1, 2].*

---

## PHẦN II: BỘ SCHEMA HỢP ĐỒNG DỮ LIỆU (DATA CONTRACT)
*(Mã nguồn TypeScript lưu tại `src/types/dataContract.ts`)[cite: 1]*

```typescript
// ==========================================
// 1. BỐI CẢNH & HOÀN CẢNH SỬ DỤNG (S2)
// ==========================================
export type EventType = 
  | 'di_chua'        // Đi lễ chùa / Không gian tâm linh (Trang nghiêm tối đa)[cite: 1]
  | 'tet_nguyen_dan' // Chúc Tết / Du xuân gia đình (Trang trọng)[cite: 1]
  | 'le_tot_nghiep'  // Lễ tốt nghiệp / Chụp kỷ yếu (Lịch sự, trang trọng)[cite: 1]
  | 'di_cafe'        // Dạo phố / Cà phê cuối tuần (Thường nhật, casual)[cite: 1]
  | 'di_bar_quay';   // Party / Sự kiện sôi động (Tối kỵ áo lễ)[cite: 1]

export type WeatherType = 
  | 'nang_nong'      // Mùa hè (Ưu tiên đũi, lụa mỏng, tay chẽn)[cite: 1]
  | 'se_lanh'        // Mùa thu đông (Ưu tiên nhiều lớp, áo tấc)[cite: 1]
  | 'mua_xuan';      // Tiết trời mát mẻ đầu năm[cite: 1]

export interface ContextState {
  eventId: EventType; //[cite: 1]
  weatherId: WeatherType; //[cite: 1]
  formalityLevel: 'formal' | 'casual' | 'party'; //[cite: 1]
}

// ==========================================
// 2. PHÂN LOẠI LAYER VÀ DANH MỤC TRANG PHỤC
// ==========================================
export type LayerCategory = 
  | 'garment'        // Áo chính (Ngũ Thân, Áo Dài, Tứ Thân, Nhật Bình)[cite: 1]
  | 'bottom'         // Quần / Thường / Váy đụp[cite: 1]
  | 'head'           // Mũ, mấn, khăn vấn, kính mát[cite: 1]
  | 'foot'           // Giày, hài thêu, guốc mộc, sneaker[cite: 1]
  | 'belt'           // Dải đới lụa, thắt lưng da[cite: 1]
  | 'waist_pendant'  // Xà tích bạc, ngọc bội hoàng gia, túi thơm[cite: 1]
  | 'bag'            // Túi mây tre, baguette bag[cite: 1]
  | 'pattern';       // Họa tiết thêu (Hoa sen, Rồng 5 móng)[cite: 1]

export interface CatalogItem {
  id: string;               // VD: 'quan_lua_trang', 'ngoc_boi_cung_dinh'[cite: 1]
  name: string;             // Tên hiển thị giao diện[cite: 1]
  category: LayerCategory;  // Nhóm layer[cite: 1]
  era: 'ly' | 'le' | 'nguyen' | 'modern'; // Niên đại lịch sử[cite: 1]
  styleCategory: 'traditional' | 'streetwear' | 'royal'; // Phong cách thiết kế[cite: 1]
  assetUrl: string;         // Đường dẫn ảnh PNG chuẩn 600x800px[cite: 1]
  thumbnailUrl: string;     // Ảnh thumbnail vuông hiển thị trên thanh chọn đồ[cite: 1]
  isRoyalOnly?: boolean;    // Cờ độc quyền hoàng gia (VD: Rồng 5 móng)[cite: 1]
  isSacredOnly?: boolean;   // Cờ cấm kỵ tâm linh/táng lễ[cite: 1]
}

// ==========================================
// 3. STATE TRUNG TÂM PHÒNG THỬ ĐỒ (S4)
// ==========================================
export interface ActiveOutfitState {
  context: ContextState; //[cite: 1]
  
  // Áo chính (Thân trên)
  garment: {
    type: 'ngu_than' | 'ao_dai' | 'tu_than' | 'nhat_binh'; //[cite: 1]
    sleeve: 'tay_chen' | 'tay_thung'; // Tay chẽn (thường phục) / Tay thụng (lễ phục)[cite: 1]
    colorId: string; // Mã màu hex hoặc định danh màu[cite: 1]
    isFlapReversed: boolean; // false = Vạt Tả đè Hữu (Chuẩn) | true = Vạt Hữu đè Tả (Lỗi Đỏ 🔴)[cite: 1]
  };

  // Quần / Thường (Thân dưới - Bắt buộc chọn)
  bottom: {
    id: string;      // ID từ catalog (VD: 'quan_lua_trang', 'quan_au_suong')[cite: 1]
    colorId: string; //[cite: 1]
  };

  // Phụ kiện đính kèm (null nếu không mang)
  accessories: {
    head: string | null;          // ID mũ/kính[cite: 1]
    foot: string | null;          // ID giày/guốc[cite: 1]
    belt: string | null;          // ID dải đới / thắt lưng[cite: 1]
    waistPendant: string | null;  // ID xà tích / ngọc bội[cite: 1]
    bag: string | null;           // ID túi xách[cite: 1]
    pattern: string | null;       // ID họa tiết thêu[cite: 1]
  };

  // Khuôn mặt người dùng ghép tại S2
  userFace: {
    isCustom: boolean;       // Có dùng ảnh upload không[cite: 1]
    imageUrl: string | null; // DataURL hoặc đường dẫn ảnh[cite: 1]
    scale: number;           // Mức độ thu phóng (0.5 đến 2.0)[cite: 1]
    offsetX: number;         // Tọa độ dịch chuyển ngang[cite: 1]
    offsetY: number;         // Tọa độ dịch chuyển dọc[cite: 1]
  };
}

// ==========================================
// 4. TRI THỨC VĂN HÓA & ĐIỂM CHẠM (HOTSPOTS)
// ==========================================
export interface HotspotDetail {
  id: string;
  title: string;
  targetPart: 
    | 'collar'        // Cổ áo[cite: 1]
    | 'sleeve'        // Ống tay[cite: 1]
    | 'button'        // Hàng khuy cúc[cite: 1]
    | 'back'          // Sống lưng áo[cite: 1]
    | 'hem'           // Tà áo / gấu quần[cite: 1]
    | 'waist'         // Cạp quần luồn dải rút[cite: 1]
    | 'belt'          // Dây thắt lưng, dải bao[cite: 1]
    | 'waist_pendant' // Ngọc bội, xà tích[cite: 1]
    | 'pant_leg'      // Ống quần xéo[cite: 1]
    | 'pleats';       // Nếp gấp váy thường[cite: 1]
  coords: { x: number; y: number }; // Tọa độ % trên Canvas 600x800px[cite: 1]
  viewAngle: 0 | 90 | 180;          // Góc hiển thị[cite: 1]
  quote: string;                    // Câu dẫn ngắn gọn[cite: 1]
  meaning: string;                  // Triết lý và ý nghĩa văn hóa[cite: 1]
  historicalContext: string;        // Bối cảnh triều đại lịch sử[cite: 1]
  modernStylingTip: string;         // Gợi ý phối thời trang ứng dụng[cite: 1]
}

export interface CostumeKnowledge {
  costumeId: string;         // Khớp với garment.type
  name: string;
  era: string;               // VD: 'Triều Nguyễn (1802 - 1945)'
  originRegion: string;      // Bắc Bộ / Huế / Nam Bộ
  significance: string;
  doAndDonts: {
    dos: string[];           // Điều nên làm
    donts: string[];         // Điều cấm kỵ
  };
  hotspots: HotspotDetail[]; // Danh sách điểm ghim tương tác[cite: 1]
}

// ==========================================
// 5. ĐẦU RA CỦA RULE ENGINE (0ms Scan)
// ==========================================
export type AlertLevel = 'RED' | 'YELLOW' | 'GREEN'; //[cite: 1]

export interface CulturalAlert {
  id: string;               // Mã định danh lỗi (VD: 'ERR_FUNERAL_FLAP')[cite: 1]
  level: AlertLevel;        // Cấp độ vi phạm[cite: 1]
  title: string;            // Tiêu đề thông báo[cite: 1]
  message: string;          // Giải thích lý do[cite: 1]
  fixSuggestion: string;    // Hướng dẫn khắc phục[cite: 1]
}

export interface RuleEngineResult {
  score: number;                 // Cultural Balance Score (0 - 100)[cite: 1]
  rankTitle: string;             // Danh hiệu (VD: 'Bậc Thầy Remix ✨')[cite: 1]
  canSaveLookbook: boolean;      // false nếu dính bất kỳ cờ RED nào[cite: 1]
  highestAlertLevel: AlertLevel; // Màu đèn chính cần hiển thị[cite: 1]
  alerts: CulturalAlert[];       // Danh sách các thông báo đang kích hoạt[cite: 1]
  activeBonusCombos: string[];   // Danh sách các combo xanh được cộng điểm[cite: 1]
}

// ==========================================
// 6. GIAO TIẾP GEMINI API STYLIST (S5)
// ==========================================
export interface GeminiStylistRequest {
  garmentName: string;        //[cite: 1]
  bottomName: string;         //[cite: 1]
  beltName?: string;          //[cite: 1]
  waistPendantName?: string;  //[cite: 1]
  formality: string;          //[cite: 1]
  event: string;              //[cite: 1]
  weather: string;            //[cite: 1]
  accessoriesList: string[];  //[cite: 1]
  culturalScore: number;      //[cite: 1]
  triggeredAlerts: string[];  //[cite: 1]
}

export interface GeminiStylistResponse {
  oneLineCritic: string;       // Đúng 1 câu nhận xét < 30 từ chuẩn tone Gen Z[cite: 1]
  suggestedHashtags: string[]; // Danh sách hashtag đề xuất cho Lookbook[cite: 1]
}