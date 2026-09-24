export type LayerCategory = 'garment' | 'color' | 'head' | 'foot' | 'bag' | 'pattern';

export interface CatalogItem {
  id: string;               // Mã định danh duy nhất (VD: 'sneaker_trang')
  name: string;             // Tên hiển thị (VD: 'Sneaker Trắng Minimalist')
  category: LayerCategory;  // Nhóm phân loại layer
  era: 'ly' | 'le' | 'nguyen' | 'modern'; // Niên đại lịch sử
  styleCategory: 'traditional' | 'streetwear' | 'royal'; // Phong cách
  
  // Tài nguyên hiển thị (Khung chuẩn 600x800px)
  assetUrl: string;         // Đường dẫn ảnh PNG trong suốt
  thumbnailUrl: string;     // Ảnh icon nhỏ cho thanh Toolbar
  
  // Cờ dữ liệu phục vụ Rule Engine
  isRoyalOnly?: boolean;    // Họa tiết/màu sắc độc quyền hoàng gia (VD: Rồng 5 móng)
  isSacredOnly?: boolean;   // Đồ chuyên dụng cho nghi lễ táng/tâm linh
}