export interface HotspotDetail {
id: string;               // VD: 'co\_ao\_chu\_nhat'
title: string;            // VD: 'Cổ Áo Chữ Nhật'
targetPart: 'collar' | 'sleeve' | 'button' | 'back' | 'hem' |'waist'|'waistpendant;
coords: { x: number; y: number }; // Tọa độ % hiển thị điểm ghim trên Canvas
viewAngle: 0 | 90 | 180;  // Góc hiển thị tương ứng
quote: string;            // Câu trích dẫn ngắn tạo điểm nhấn
meaning: string;          // Ý nghĩa văn hóa / triết lý cấu tạo
historicalContext: string;// Bối cảnh xuất hiện trong lịch sử
modernStylingTip: string; // Mẹo phối đồ hiện đại
}

export interface CostumeKnowledge {
costumeId: string;        // Khớp với garment.type
name: string;
era: string;              // VD: 'Triều Nguyễn (1802 - 1945)'
originRegion: string;     // Xuất xứ: Bắc Bộ / Huế / Nam Bộ
significance: string;     // Ý nghĩa tổng thể
doAndDonts: {
dos: string\[];          // Những điểm nên tuân thủ
donts: string\[];        // Các đại kỵ cần tránh
};
hotspots: HotspotDetail\[];// Danh sách điểm chạm giải mã
}

