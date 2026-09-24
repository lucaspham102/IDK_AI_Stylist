// Gửi từ Client lên API (Payload)
export interface GeminiStylistRequest {
  garmentName: string;
  formality: string;
  event: string;
  weather: string;
  accessoriesList: string[];
  culturalScore: number;
  triggeredAlerts: string[];
}

// Dữ liệu phản hồi từ Gemini API (Response)
export interface GeminiStylistResponse {
  oneLineCritic: string;       // Đúng 1 câu nhận xét chuẩn giọng điệu Gen Z (< 30 từ)
  suggestedHashtags: string[]; // Danh sách hashtag đề xuất cho Lookbook
}