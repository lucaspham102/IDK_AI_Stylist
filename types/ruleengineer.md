export type AlertLevel = 'RED' | 'YELLOW' | 'GREEN';

export interface CulturalAlert {
  id: string;               // Mã cảnh báo (VD: 'ERR_FUNERAL_FLAP')
  level: AlertLevel;        // Mức độ cảnh báo
  title: string;            // Tiêu đề cảnh báo
  message: string;          // Nội dung giải thích chi tiết
  fixSuggestion: string;    // Hướng dẫn khắc phục nhanh
}

export interface RuleEngineResult {
  score: number;            // Điểm Cultural Balance Score (0 - 100)
  rankTitle: string;        // Danh hiệu đạt được
  canSaveLookbook: boolean; // false nếu dính lỗi ĐỎ (Khóa lưu ảnh)
  highestAlertLevel: AlertLevel; // Cấp độ đèn chủ đạo (🔴 / 🟡 / 🟢)
  alerts: CulturalAlert[];  // Danh sách các thông báo vi phạm/nhắc nhở
  activeBonusCombos: string[]; // Danh sách combo sáng tạo được cộng điểm
}