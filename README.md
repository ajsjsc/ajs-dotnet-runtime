# ajs-dotnet-runtime

Nguồn tải runtime .NET cho các ứng dụng AJS (B2E, ajsCAD, AJS Cowork…). Nguyên tắc chọn runtime được mô tả ở ajsCAD
`docs/DESKTOP-PRODUCTS.md`, mục 6.1. Thứ tự ưu tiên:

1. runtime .NET của hệ thống;
2. runtime chung `%LOCALAPPDATA%\AJS\dotnet`;
3. nếu thiếu, giải nén gói nhúng trong bộ cài hoặc tải gói.

Mỗi release `v<phiên bản>` chứa **nguyên tệp chính thức của Microsoft**, không đóng gói lại:

| Tệp | Dùng cho |
|---|---|
| `dotnet-runtime-<ver>-win-x64.zip` | runtime chung: `dotnet.exe`, `host\fxr`, `shared\Microsoft.NETCore.App` |
| `windowsdesktop-runtime-<ver>-win-x64.zip` | runtime chung: `shared\Microsoft.WindowsDesktop.App` (giải nén chồng lên zip trên) |
| `windowsdesktop-runtime-<ver>-win-x64.exe` | cài cho toàn máy (`AJS_Runtimes_Setup.exe /machine`) |

- **Kiểm toàn vẹn:** thư viện `AJS.Runtime` (repo ajs-web-ecosystem, `Shared/AJS.Runtime`) kiểm SHA-512 do Microsoft công bố trong
  [`releases.json`](https://builds.dotnet.microsoft.com/dotnet/release-metadata/10.0/releases.json).
- **Nguồn dự phòng:** nếu không tải được từ đây, ứng dụng tải thẳng từ `builds.dotnet.microsoft.com`.
- **Giấy phép:** .NET phát hành theo giấy phép MIT. `LICENSE.txt` và `ThirdPartyNotices.txt` có sẵn trong từng zip.

## Phát hành bản vá

Khi Microsoft ra bản vá, chạy workflow **Đồng bộ runtime .NET** (`sync-runtime.yml`) với phiên bản cần lấy (vd `10.0.16`).

Workflow làm các việc sau:
1. tải ba tệp;
2. đối chiếu SHA-512 với `releases.json`;
3. tạo release `v<ver>` kèm bảng SHA-512.

Sau đó nâng `RuntimeRelease.Current` trong `AJS.Runtime`, rồi build lại stub, launcher và `AJS_Runtimes_Setup.exe`.
