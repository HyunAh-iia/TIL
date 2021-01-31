# Firebase Storage
https://firebase.google.com/docs/reference/js/firebase.storage

이미지 업로드, 삭제
```
FileList {0: File, length: 1}
0: File
    lastModified: 1611756885337
    > lastModifiedDate: Wed Jan 27 2021 23:14:45 GMT+0900 (대한민국 표준시) {}
    name: "스크린샷 2021-01-27 오후 11.14.39.png"
    size: 58694
    type: "image/png"
    webkitRelativePath: ""
    > __proto__: File
length: 1
> __proto__: FileList
```

fileReader API (javascript API임)
---
[fileReader API 정리](../../javascript/fileReader-API/fileReader-API.md)
이거는 파일 프리뷰할때 씀 :)

filebase stora


ref mehotd : https://firebase.google.com/docs/reference/js/firebase.storage.Storage#ref
반환값 Reference : 구글 클라우드 스토리지 오브젝트에 대한 참조
child를 만들고싶오?!
child는 기본적으로 이미지의 패스를 넣엇!! Collection이랑 비슷해!!
reference에서 폴더를 만들 ㅜㅅ 이써!
모든 유저의 사진은 아이디와 분리! 어어!그래!그래
유저 아이디로 폴더를 만드는거구나! ㅇㅋ


파일삭제!
- refFromURL : 버킷에서 반환된 public URL을 통해 Reference 객체 찾기
- 그 후에 딜리뜨!