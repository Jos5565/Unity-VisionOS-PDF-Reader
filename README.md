## Unity VisionOS Pdf Renderer

> 기본 에셋 : [Paraxe/PDF Reader AssetStore](https://assetstore.unity.com/packages/tools/gui/pdf-renderer-32815)
>
>  직접 만든 이유 : PDF Renderer 가 Vision OS 를 타겟한 Plugin(.dll)을 제공하지 않기에.

## 작업 방법
> "Goolge pdfium" 을 타겟으로 라이브러리가 구성되어 있는 것을 확인.
> 
> pdfium의 ios ARM64 라이브러리를 활용하여 환경구성을 "visionos" 로 변경하여 빌드 한다면, 같은 ARM64 환경의 Vision os에서도 사용 할 수 있을 것 같다는 생각이 듬.
>
> Ninja 를 이용한 Static Library 빌드 시작.
>
> 빌드된 Static Library를 Unity 환경에 적용하여, Vision Pro 에서 구동되는 것을 확인.

## 적용 방법 
> 1. 기존 PDF Renderer/Paroxe/PDFRenderer/Plugins/ 경로에 "VisionOS" 폴더 추가 후, .a Library 파일을 해당 폴더에 삽입.
>
> <img width="494" height="229" alt="image" src="https://github.com/user-attachments/assets/fc0eaa81-2593-4022-bf39-911c6d001eb4" />
>
> 2. Asset Import Setting 에서 "VisionOS" 를 선택 
> <img width="494" height="201" alt="image" src="https://github.com/user-attachments/assets/cd76096b-608a-43a7-81cb-cea427fe9800" />
>
>  3. 라이브러리의 CPU 타겟을 ARM64로 선택
> <img width="475" height="770" alt="image" src="https://github.com/user-attachments/assets/f8c17b6a-05f5-4d25-ab15-5a1abcef4c74" />
>
> 4. PlayerSetting/Player의 세팅을 다음 사진과 동일하게 설정
> <img width="785" height="325" alt="image" src="https://github.com/user-attachments/assets/254465ef-2816-4ce1-938f-2c0a4c9f5283" />



