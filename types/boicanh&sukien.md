# 📜 BỘ SCHEMA HỢP ĐỒNG DỮ LIỆU (DATA CONTRACT) - VIỆT PHỤC REMIX

Tài liệu chuẩn hóa cấu trúc dữ liệu dùng chung giữa Frontend (State / UI Canvas) và Backend / Logic Engine (Rules & API).

---

## 1. Schema Bối Cảnh & Sự Kiện (ContextState)
Áp dụng cho luồng S2 và ràng buộc logic tại S4:

```typescript
export type EventType = 
  | 'di_chua'        // Đi lễ chùa / Tâm linh (Yêu cầu trang nghiêm tuyệt đối)
  | 'tet_nguyen_dan' // Chúc Tết / Du xuân (Lễ hội gia đình)
  | 'le_tot_nghiep'  // Chụp kỷ yếu / Lễ tốt nghiệp (Trang trọng)
  | 'di_cafe'        // Dạo phố / Cà phê cuối tuần (Thường nhật)
  | 'di_bar_quay';   // Party / Sự kiện sôi động (Tối kỵ áo tế, lễ phục)

export type WeatherType = 
  | 'nang_nong'      // Mùa hè (Phù hợp vải nhẹ, đũi, tay chẽn)
  | 'se_lanh'        // Mùa thu đông (Phù hợp áo tấc, nhiều lớp)
  | 'mua_xuan';      // Mát mẻ đầu năm

export interface ContextState {
  eventId: EventType;
  weatherId: WeatherType;
  formalityLevel: 'formal' | 'casual' | 'party';
}