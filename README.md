## Unity VisionOS Pdf Renderer

> 기본 에셋 : [Paraxe/PDF Reader AssetStore](https://assetstore.unity.com/packages/tools/gui/pdf-renderer-32815)
> 정상적인 구동을 위해서는 위의 에셋을 구매 후 활용해야함.
>
>  직접 만든 이유 : PDF Renderer 가 Vision OS 를 타겟한 Plugin(.dll)을 제공하지 않기 때문에.
>


## 작업 진행
> "Goolge pdfium" 을 타겟으로 라이브러리가 구성되어 있는 것을 확인.
> 
> pdfium의 ios ARM64 라이브러리를 활용하여 환경구성을 "visionos" 로 변경하여 빌드 한다면, 같은 ARM64 환경의 Vision os에서도 사용 할 수 있을 것 같다는 생각이 듬.
>
> Ninja 를 이용한 Static Library 빌드 시작.
>
> 빌드된 Static Library를 Unity 환경에 적용하여, Vision Pro 에서 성공적으로 작동하는 것을 확인함. 

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


## Library Build Process
> 
pdfium 폴더로 이동 후. 
### [Tool Setup]
export PATH="$PATH:{LOCAL PATH}/depot_tools"

### [Build libpdfium.a]
1. Build Setting gn 명령어
 
> gn gen out --args="target_os=\"ios\" target_cpu=\"arm64\" target_environment=\"visionos\" use_custom_libcxx=true pdf_use_partition_alloc=false pdf_enable_v8=false pdf_enable_xfa=false pdf_is_standalone=true pdf_is_complete_lib=true is_debug=false use_blink=false ios_enable_code_signing=false use_partition_alloc=false use_allocator_shim=false "

2. pdfium 라이브러리 생성

> ninja -C out pdfium

### [Build libc++.a]
1. libc++ .o 파일 만들기

> ninja -C out libc++  

2. libc++ 빌드에 사용된 모든 객체 파일(.o)을 찾아서 리스트업
   
> find obj/buildtools/third_party/libc++ -name "*.o" > libcxx_files.txt

3. 찾은 .o 파일들을 하나의 정적 라이브러리로 묶기
   
> libtool -static -o libc++_final.a -filelist libcxx_files.txt

### [Build libc++abi.a]
1. 객체 파일 리스트업
   
> find obj/buildtools/third_party/libc++abi -name "*.o" > libcxxabi_files.txt

2. 진짜 라이브러리로 병합

> libtool -static -o libc++abi_final.a -filelist libcxxabi_files.txt
