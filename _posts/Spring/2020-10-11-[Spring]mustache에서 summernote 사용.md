---
title: "[Spring] mustache에서 summernote 사용"
tags:
  - Spring
date: 2020-10-11T08:06:00-05:00
key: "[Spring] mustache에서 summernote 사용"
---

# summernote + mustache

웹에디터인 summernote 글을 작성하고, 스프링 컨트롤러를 통해 데이터를 전달해서 mustache에서 작성한 글을 확인해본다.<br>

<!--more-->

## summernote 연결 및 생성

### import

```html
<!-- include libraries(jQuery, bootstrap) -->
<link href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" rel="stylesheet">
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>

<!-- include summernote css/js -->
<link href="https://cdn.jsdelivr.net/npm/summernote@0.8.18/dist/summernote.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/summernote@0.8.18/dist/summernote.min.js"></script>
```

summernote 사용 시에 필요한 라이브러리들을 우선 임포트 해 준다.<br>

### summer note 클래스 지정

```html
<div class="form-group">
  <label for="content">ToDo</label>
  <textarea class="form-control summernote" id="content"></textarea>
</div>
```
위와 같이 부트스트랩에서 summernote로 사용될 클래스를 만들어주고, 클래스에 `summernote`를 추가한다.<br>

```html
<script>
    $('.summernote').summernote({
                height:250
    });
</script>
```
스크립트 태그에 서머 노트를 사용할 수 있게 추가해 준다.<br>
`height` 속성으로 크기를 필요한 만큼 지정해 준다.<br>

### summernote 화면

![1](/assets/images/201011-1.png)<br>
위와 같이 프론트에서 서머 노트를 사용할 수 있다.<br>

## ajax로 연결

위의 이미지처럼 버튼을 하나 만들고, ajax로 스프링 컨트롤러와 연결해 준다.<br>

```javascript
    save: function (){
        let data = {
            title: $("#title").val(),
            content: $("#content").val(),
        };
        if(data.title==""){
            alert("title을 작성해주세요.");
            return;
        }
        $.ajax({
            type:"POST",
            url:"/api/writeProc",
            data: JSON.stringify(data),
            contentType:"application/json;utf-8",
        })
        .done(function (response){
            if(response.status===500){
                alert("작성에 실패하였습니다.");
            }
            else{
                alert("작성에 성공하였습니다.");
            }
            location.href="/todolist/proceeding?page=0";
        })
        .fail(function (error){
           JSON.stringify(error);
        })
    }
```

이제 `"/api/writeProc"`의 주소로 스프링 컨트롤러와 연결해 주자.<br>

## api controller & service

### api controller
```java
    @PostMapping("api/writeProc")
    public int write(@RequestBody TaskWriteDto taskWriteDto, @AuthenticationPrincipal PrincipalDetails principalDetails){
        taskService.write(taskWriteDto, principalDetails);
        return HttpStatus.OK.value();
    }
```

### api service
```java
    @Transactional
    public void write(TaskWriteDto taskWriteDto, PrincipalDetails principalDetails){
        taskRepository.save(taskWriteDto.toEntity());
    }
```

게시글 작성에 필요한 dto를 따로 생성해 줬고, 컨트롤러와 서비스를 사용해 글을 db에 저장한다.<br>
이때 서비스에는 트랜잭션을 보장하기 위해 @Transactional 어노테이션을 붙여준다.<br>

### JSON 형태로 결과 확인

mustache로 출력하기 전, 우선 데이터가 잘 전달되는지 JSON 형태로 확인해보자.<br>

![2](/assets/images/201011-2.png)<br>

위와 같은 test 글을 작성해보자.<br>

```java
   @GetMapping("/task/detail/{id}")
    @ResponseBody
    public Task taskDetail(@PathVariable long id){
        Task requestTask = taskService.findTask(id);
        return requestTask;
    }
```

위와 같이 @ResponseBody를 이용해 데이터를 반환하는 컨트롤러를 만들었다.<br>

![3](/assets/images/201011-3.png)<br>

데이터가 잘 전달됨을 확인할 수 있다.<br>

## mustache로 출력

```html
<br/>
<div class="container">
    <h1>{{task.title}}</h1>
    <hr/>
    <div>{{{task.content}}}</div>
</div>
```

model 객체로 데이터를 전송해 주고 mustache의 body 부분만 보면 다음 같이 작성할 수 있다.<br>
**여기서 주의할 점은, summernote의 content 부분은 중괄호가 세 쌍으로 들어가야 한다!!** <br>
두 쌍으로 들어가면 mustache가 html 소스로 해석하지 않고 다시 컴파일하여 html의 내용을 text로 출력해 준다.<br>

### 두 개의 중괄호 일 때
![4](/assets/images/201011-4.png)<br>

### 세 개의 중괄호 일 때
![5](/assets/images/201011-5.png)<br>

세 개의 중괄호로 정상적으로 summernote의 content를 출력하는 것을 알 수 있다.<br>
