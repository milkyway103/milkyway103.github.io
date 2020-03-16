---
layout: post
title: 자바로 특정 폴더의 이미지 파일 리스트를 불러와 이름 변경하기
categories : java
tags : java FileIO
comments : true
---

## 개요

마크다운을 이용하여 깃허브 블로그에 올릴 때, 스크린샷을 찍고 파일명을 변경하고, 폴더에 복사 및 붙여넣기하는 일련의 과정이 꼭 필요하면서도 시간이 소요되는 반복 업무였다는 것을 깨달았다.

그래서 간단한 fileIO를 하면 되는 일이니, 자바를 이용해서 만들어 보기로 결정했다.

## 요구사항

1. 폴더로부터 이미지 파일 리스트 읽기
2. 이미지 파일 이름 변경하기
3. 아톰 `assets/post-img` 폴더에 저장하기
4. markdown에 쓸 html 생성하기

저장 폴더나, 이미지 이름 저장 양식 등은 통일되는 것이었기 때문에 꼭 필요한 것만 그때그때 입력받기로 했다.
> 저장될 파일명 (예 : javascriptstudy-image)
> 시작하는 숫자 (하나의 포스팅을 이어서 쓸 경우 필요하다.)
> 아톰에서 저장될 경로 (예 : javasctipt 폴더)

## 완성된 코드

```java
package imagenaming;

import java.io.File;
import java.io.FileFilter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class readimage {


	public static void main(String[] args) throws IOException {

		Scanner sc = new Scanner(System.in);

		String screenshotpath = "/Users/eunha/desktop/screenshot";
		// 저장하고자 하는 문자열
		System.out.println("바꾸고자 하는 이미지 파일명을 주제-imge 형태로 입력하세요.");
		String basename = sc.nextLine();
		// 시작하는 숫자
		int startnum;
		System.out.println("시작하는 숫자를 입력하세요.");
		startnum = sc.nextInt();
		String basepath = "/Users/eunha/desktop/milkyway103.github.io";
		System.out.println("아톰에서 저장될 경로를 입력하세요.");
		String savepath = sc.next();
		savepath = "/assets/post-img/" + savepath + "/";

		// 이미지 목록을 가져올 폴더 위치를 지정 (절대경로, 상대경로 모두 가능)
		List<File> list = getImgFileList(screenshotpath);

		// 오름차순 정렬
		list.sort(null);

		for (File f : list) {
			// 이름 변경
			File file = new File(screenshotpath + "/" + f.getName());
			String savename = basename + Integer.toString(startnum++) + ".png";
			file.renameTo(new File(basepath + savepath + savename));
			System.out.println("<center>\n" +
					"   <figure>\n" +
					"   <img src=\"" +
					savepath +
					savename +
					"\" alt=\"views\">\n" +
					"   <figcaption></figcaption>\n" +
					"   </figure>\n" +
					"</center>"); // 마크다운용 html 출력
		}

	}

  // 해당 경로의 이미지파일 목록 반환
  public static List<File> getImgFileList(String path){
  	return getImgFileList(new File(path));
  }

  // 해당 경로의 이미지 파일 목록 반환
  public static List<File> getImgFileList(File file){
  	List<File> resultList = new ArrayList<File>(); // 이미지 파일을 저장할 리스트 생성
  	// 지정한 이미지 폴더가 존재하지 않을 경우 빈 리스트 반환
  	if(!file.exists()) return resultList;

  	File[] list = file.listFiles(new FileFilter() { // 원하는 파일만 가져오기 위해 FileFilter 정의
  		String strImgExt="jpg|jpeg|png|gif|bmp"; // 허용할 이미지타입

  		@Override
  		public boolean accept(File pathname) {

  			boolean chkResult = false;
  			if(pathname.isFile()) {
  				String ext = pathname.getName().substring(pathname.getName().lastIndexOf(".")+1);
  				chkResult = strImgExt.contains(ext.toLowerCase());
  			} else {
  				chkResult = true;
  			}
  			return chkResult;
  		}

  	});

  	for (File f : list) {
  		if (f.isDirectory()) {
  			// 폴더이면 이미지 목록을 가져오는 현재 메서드를 재귀 호출
  			resultList.addAll(getImgFileList(f));
  		} else {
  			// 폴더가 아니고 파일이면 리스트(resultList)에 추가
  		resultList.add(f);
  		}
  	}

  	if (resultList.isEmpty()) {
  		System.out.println("This directory is empty");
  	}

  	return resultList;
  }
}
```

<center>
   <figure>
   <img src="/assets/post-img/java/filerename-image1.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

<center>
   <figure>
   <img src="/assets/post-img/java/filerename-image2.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

좀더 사용하기 편리하도록 GUI 방식으로도 만들어 보아야겠다.

## 참고
- [[Java] 특정 경로에 있는 이미지파일 목록을 불러오자](https://m.blog.naver.com/PostView.nhn?blogId=javaking75&logNo=220116830853&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
- [JAVA 파일 이름 변경, 확장자 변경 코드](https://aiden1004.tistory.com/entry/JAVA-%ED%8C%8C%EC%9D%BC-%EC%9D%B4%EB%A6%84-%EB%B3%80%EA%B2%BD-%ED%99%95%EC%9E%A5%EC%9E%90-%EB%B3%80%EA%B2%BD-%EC%BD%94%EB%93%9C)
