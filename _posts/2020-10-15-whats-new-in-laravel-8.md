---
layout: post
title: "라라벨 8의 새로운 기능"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/laravel.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/laravel.png?fit=730%2C412&ssl=1)

라라벨 8은 2020년 9월 8일에 출시되었다. 이 릴리스는 이전 릴리스(버전 7)에서 개선된 기능뿐만 아니라 Jetstream, 작업 배치, 동적 블레이드 구성 요소, 모델 팩토리 클래스, 개선된 장인 서비스 등을 포함한 새로운 기능을 계속 제공합니다.

이 기사에서는 아래 나열된 바와 같이 이번 새 릴리스에 도입된 13가지 새로운 기능에 대해 살펴보겠습니다.

- 라라벨 제트스트림
- 모델 디렉토리
- 모델 팩토리 클래스
- 마이그레이션 스쿼시
- 작업 배치
- 요금제한 개선
- 향상된 유지 보수 모드
- 폐쇄 디스패치/체인
- 다이내믹 블레이드 구성 요소
- 시간 테스트 도우미
- 장인 서비스 개선
- 순풍 페이지 뷰
- 네임스페이스 업데이트 라우팅

## 라라벨 제트스트림

라라벨 제트스트림(Laravel Jetstream)은 라라벨 애플리케이션을 위한 아름답게 조작된 애플리케이션이다. 테일윈드 CSS를 활용해 디자인된 제트스트림은 라라벨 생텀을 활용한 인증, 프로필 관리, 보안, API 지원 등의 기능을 갖춘 신규 프로젝트의 완벽한 출발점을 제공한다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/laraveljetstream.png?resize=730%2C550&ssl=1)

또한 Jetstream은 Livewire와 Initious를 사용하여 프런트 엔드 비계를 위한 두 가지 옵션을 제공합니다.

Laravel Livewire는 Retrove와 Vue.js와 같은 다른 프런트 엔드 라이브러리/프레임워크를 가져올 필요 없이 Laravel에 풀 스택 애플리케이션을 구축할 수 있게 해주는 라이브러리이다. 라이브와이어는 이미 익숙한 혼합 템플릿 엔진을 사용하기 때문에 라라벨 개발자들은 라라벨의 편안함을 떠나지 않고도 쉽게 동적 인터페이스를 구축할 수 있다.

Initarity.js — Vue.js를 사용하여 클라이언트측 템플릿을 빠르게 구축할 수 있는 Laravel Jetstream과 번들된 패키지입니다. 이 멋진 점은 Frontend 라우팅의 복잡성 없이 Vue의 모든 파워를 즐길 수 있다는 것입니다. 왜냐하면 익숙한 표준 Laravel 라우터를 사용하기 때문입니다.

Jetstream 설치 - Laravel 설치 관리자가 설치된 경우 다음과 같은 `--jet` 플래그를 추가하여 Laravel 설치와 함께 Jetstream을 쉽게 설치할 수 있습니다.

```shell
$ laravel new project-name --jet
```

마이그레이션을 실행하여 설정을 완료합니다.

```shell
$ php artisan migrate
```

또는 Composer를 사용하여 Jetstream을 새 Laravel 애플리케이션에 설치할 수 있습니다. 작곡가를 통해 Jetstream을 설치하려면 선호하는 프런트 엔드 스택(예: glivewire 또는 Initious.js)의 이름을 허용하는 `jetstream:install` 아티산 명령을 실행해야 합니다. 이 작업은 아래 명령을 실행하여 수행할 수 있습니다.

```shell
$ php artisan jetstream:install livewire

$ php artisan migrate

$ npm install && npm run dev
```

자세한 내용은 Jetstream 공식 설명서를 참조하십시오.

## 모델 디렉토리

라라벨이 모델을 저장하기 위한 기본값으로 `모델` 디렉토리를 가져야 한다는 제안이 항상 제기되어 왔다. 2016년 Taylor Otwell은 이에 대한 설문조사를 실시했고, 그 결과 기본 모델 디렉토리를 원하는 비율이 더 높은 것으로 나타났다. 4년 후 국민의 요청이 받아들여졌다.

이전 버전의 Laravel에서는 모델을 생성할 때 경로를 지정하지 않으면 기본적으로 모든 모델 파일이 `/app` 디렉토리에 저장됩니다. 그러나 새로운 업데이트 이후, Laravel은 기본적으로 `app/Models` 디렉토리를 포함합니다.

따라서 `$pphartisan make:modelName` 명령을 실행하면 `모델 이름`이 실행됩니다.php는 `앱/모델`에 저장됩니다. 그러나 디렉토리가 존재하지 않는 경우, Laravel은 애플리케이션 모델이 이미 `app/` 디렉토리에 있다고 가정합니다.

## 모델 팩토리 클래스

웅변 모델 공장에서는 응용 프로그램을 테스트할 때 가짜 데이터를 생성하는 데 사용되는 패턴을 정의할 수 있습니다. 이전 버전에서 라라벨은 공장을 정의하기 위해 확장할 수 있는 `$factory` 글로벌 객체를 제공한다. Laravel 8부터, 공장은 이제 공장 간의 관계에 대한 향상된 지원과 함께 클래스 기반입니다(즉, 사용자는 많은 게시물을 가지고 있다.

이전에 공장 정의는 다음과 같습니다.

```php
// database/factories/UserFactory.php

use Faker\Generator as Faker;
use Illuminate\Support\Str;

$factory->define(App\User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'email_verified_at' => now(),
        'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
        'remember_token' => Str::random(10),
    ];
});
```

그러면 다음과 같이 정의된 공장을 사용할 수 있습니다.

```php
public function testDatabase()
{
    $user = factory(App\User::class)->make();

    // Use model in tests...
}
```

새 버전 이후 공장에서는 다음과 같이 클래스로 정의됩니다.

```php
// database/factories/UserFactory.php

namespace Database\Factories;
use App\Models\User;
use Illuminate\Database\Eloquent\Factories\Factory;
use Illuminate\Support\Str;

class UserFactory extends Factory
    {
        /**
         * The name of the factory's corresponding model.
         *
         * @var string
         */
        protected $model = User::class;

        /**
         * Define the model's default state.
         *
         * @return array
         */
        public function definition()
        {
            return [
                'name' => $this->faker->name,
                'email' => $this->faker->unique()->safeEmail,
                'email_verified_at' => now(),
                'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
                'remember_token' => Str::random(10),
            ];
        }
    }
```

생성된 모델에서 사용할 수 있는 새로운 "HasFactory" 특성이 있는 모델 공장은 다음과 같이 사용될 수 있습니다.

```php
use App\Models\User;
public function testDatabase()
{
    $user = User::factory()->make();
    // Use model in tests...
}
```

## 마이그레이션 스쿼시

대용량 마이그레이션 파일을 하나의 SQL 파일로 압축할 수 있는 새로운 마이그레이션 스쿼시 기능을 통해 비대해진 마이그레이션 폴더와 작별 인사를 나눌 수 있습니다. 마이그레이션이 실행될 때 생성된 파일이 먼저 실행된 다음, Squashed 스키마 파일의 일부가 아닌 다른 마이그레이션 파일을 실행합니다. 아래의 아티스한 명령을 사용하여 마이그레이션 파일을 스쿼시할 수 있습니다.

```shell
$ php artisan schema:dump

// Dump the current database schema and prune all existing migrations...
$ php artisan schema:dump --prune
```

명령을 실행하면 Laravel이 스키마 파일을 `database/schema` 디렉토리에 씁니다.

## 작업 배치

Laravel의 새 릴리스에는 병렬로 실행할 작업 그룹을 보낼 수 있는 고급 기능도 포함되어 있습니다. 그룹화된/배치된 작업의 진행률을 모니터링하려면 다음과 같이 완료 콜백을 정의하기 위해 `then`, `catch` 및 `finally` 방법을 사용할 수 있습니다.

```php
use App\Jobs\ProcessPodcast;
use App\Podcast;
use Illuminate\Bus\Batch;
use Illuminate\Support\Facades\Batch;
use Throwable;

$batch = Bus::batch([
    new ProcessPodcast(Podcast::find(1)),
    new ProcessPodcast(Podcast::find(2)),
    new ProcessPodcast(Podcast::find(3)),
    new ProcessPodcast(Podcast::find(4)),
    new ProcessPodcast(Podcast::find(5)),
])->then(function (Batch $batch) {
    // All jobs completed successfully...
})->catch(function (Batch $batch, Throwable $e) {
    // First batch job failure detected...
})->finally(function (Batch $batch) {
    // The batch has finished executing...
})->dispatch();

return $batch->id;
```

새 작업 배치 기능에 대한 자세한 내용은 Laravel 문서를 참조하십시오.

## 요금제한 개선

새로운 개선된 요금 제한으로 이제 "요금 제한자"의 면(예: 동적 요청 제한)을 사용하여 더 많은 작업을 수행할 수 있습니다. 먼저 이전 버전에서 요청 조절이 어떻게 처리되었는지 살펴보겠습니다.

Laravel 7에서는 API 요청을 제한하려면 `Kernel`을 편집해야 합니다.`app/Http` 폴더의 php 파일:

```undefined
// app/Http/Kernel.php
...

protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],
    'api' => [
        'throttle:60,1', // Here the API request limit is set to 60 request per minute
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],
];

...
```

이제 Laravel 8에서 위의 구성은 다음과 같습니다.

```coffeescript
// app/Http/Kernel.php
...
protected $middlewareGroups = [
        'web' => [
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            // \Illuminate\Session\Middleware\AuthenticateSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],
        'api' => [
            'throttle:api', // Request limit is now defined in RouteServiceProvider
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],
    ];
...
```

API 요청 제한은 현재 `RouteServiceProvider`에서 정의되고 있습니다.php는 `app/Providers/` 디렉토리에 있습니다. 어떻게 하는지 보자:

```php
// app/Providers/RouteServiceProvider.php
use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Support\Facades\RateLimiter;
...
public function boot()
{
    $this->configureRateLimiting();
    ...
}
// Configure the rate limiters for the application.
protected function configureRateLimiting()
{
    RateLimiter::for('api', function (Request $request) {
    return Limit::perMinute(60); // 60 Request per minute
    });
}
```

boot 방식에서는 configureRateLimiting()이 호출된다. 그리고 이름에서 알 수 있듯이, 그것은 속도 제한을 위한 구성을 가지고 있습니다.

속도 제한은 속도 제한자 파사드의 "for" 방식을 사용하여 정의된다. for 방법은 두 가지 매개 변수, 즉 속도 제한 장치 이름(즉, `api`)과 이 속도 제한 장치가 할당된 경로에만 적용되는 제한 구성을 반환하는 폐쇄를 허용합니다.

보시다시피, for 방법은 HTTP 요청 인스턴스를 사용하여 요청을 동적으로 제한하는 모든 제어 권한을 부여합니다.

인증되지 않은 사용자의 경우 분당 10회 요청 제한을 설정하고 인증되지 않은 사용자의 경우 무제한 요청 제한을 설정합니다. 우리는 그렇게 할 것이다:

```php
// app/Providers/RouteServiceProvider.php

protected function configureRateLimiting()
{
    ...
    RateLimiter::for('guest', function (Request $request) {
    return $request->user()
                ? Limit:none()
                : Limit::perMinute(10); // 10 Request per minute
    });
}
```

구성된 속도는 다음과 같은 미들웨어를 사용하여 경로에 직접 적용할 수도 있습니다.

```php
// routes/api.php
...
Route::get('posts', 'PostController@store')->middleware('throttle:guest');
...
```

속도 제한에 대한 자세한 내용은 Laravel 라우팅 설명서를 참조하십시오.

## 향상된 유지 보수 모드

이전 Laravel 버전에서는 애플리케이션에 액세스할 수 있는 화이트리스트 IP 주소 목록을 설정하여 유지 보수 모드를 무시할 수 있으며, 이 기능은 `비밀/토큰`을 위해 제거되었습니다. 작동 방식을 살펴보겠습니다.

이제 응용 프로그램을 유지 보수 모드로 설정할 때 다음과 같이 사이트에 액세스하는 데 사용할 수 있는 암호를 지정할 수 있습니다.

```shell
$ php artisan down --secret="my-secret"
```

응용 프로그램이 유지 보수 모드에 있는 동안 다음과 같이 암호를 지정하여 응용 프로그램에 액세스할 수 있습니다.

https://your-website.com/my-secret

그런 다음 Laravel은 `larabel_maintenance` 키를 사용하여 쿠키를 브라우저에 설정합니다.

## 유지보수선행

유지 보수 모드의 또 다른 개선 사항은 선택한 유지 보수 보기를 미리 지정할 수 있다는 점입니다. 이전 버전의 Laravel에서는 애플리케이션이 유지 보수를 위해 중단된 동안 `composer install`을 실행하는 종속성을 업데이트하면 방문자가 실제 서버 오류를 얻을 수 있습니다.

애플리케이션의 유지보수 여부를 확인하기 위해 라라벨의 상당 부분을 부팅해야 하기 때문이다. 유지 관리 프리렌더는 요청 주기의 맨 처음에 반환되는 보기를 지정할 수 있어 유용합니다. 그러면 응용프로그램의 종속성이 로드되기 전에 이 보기가 렌더링됩니다.

다음과 같은 `artisan down` 명령의 `--ender` 옵션으로 기본 보기를 지정할 수 있습니다.

```shell
$ php artisan serve
// Starting Laravel development server: http://127.0.0.1:8000
...

$ php artisan down --render="errors::503"
// Application is now in maintenance mode.
```

위 명령을 실행하면 아래 화면이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/503error.png?resize=730%2C466&ssl=1)

## 폐쇄 디스패치/체인

이제 새 `catch` 메서드를 사용하면 구성된 모든 대기열을 모두 완료한 후 대기 중인 폐쇄가 성공적으로 완료되지 않을 경우 실행되어야 하는 폐쇄를 제공하고 다음과 같은 시도를 다시 시도할 수 있습니다.

```php
use Throwable;

dispatch(function () use ($podcast) {
    $podcast->publish();
})->catch(function (Throwable $e) {
    // This job has failed...
});
```

## 다이내믹 블레이드 구성 요소

런타임에 보기에서 수행된 작업에 따라 달라지는 구성 요소를 렌더링하려는 경우가 있습니다. 동적 블레이드 구성 요소를 사용하면 구성 요소 이름을 다음과 같은 변수로 전달하여 구성 요소를 렌더링할 수 있습니다.

```undefined
<x-dynamic-component :component="$componentName" class="mt-4" />
```

## 시간 테스트 도우미

Ruby on Rails에서 영감을 받아, 카본 PHP 라이브러리를 통한 시간 수정은 테스트 시 이동 측면에서 한 단계 더 나아갔다.

시험사례 작성 시 현재나 일루미네이트 등 도우미가 반납한 시간을 수정해야 하는 경우가 있다.서포트\Carbon:지금() 이제 Laravel의 기본 기능 테스트 클래스는 다음과 같이 현재 시간을 조작할 수 있는 도우미 메소드를 포함합니다.

```php
public function testTimeCanBeManipulated()
{
    // Travel into the future...
    $this->travel(5)->milliseconds();
    $this->travel(5)->seconds();
    $this->travel(5)->minutes();
    $this->travel(5)->hours();
    $this->travel(5)->days();
    $this->travel(5)->weeks();
    $this->travel(5)->years();

    // Travel into the past...
    $this->travel(-5)->hours();

    // Travel to an explicit time...
    $this->travelTo(now()->subHours(6));

    // Return back to the present time...
    $this->travelBack();
}
```

## 장인 서비스 개선

이전 버전의 Laravel에서는 `partison serve` 명령으로 애플리케이션을 시작할 때 `.env`를 수정하려면 애플리케이션을 수동으로 다시 시작해야 합니다. 새 버전 이후 `.env`를 수정하면 응용 프로그램이 자동으로 다시 로드되므로 응용 프로그램을 수동으로 다시 시작할 필요가 없습니다.

## 순풍 페이지 뷰

라라벨의 페이지네이터는 기본적으로 Tailwind CSS 프레임워크를 사용하도록 업데이트되었다. 여전히 부트스트랩 3과 4를 지원하고 있습니다.

기본 Tailwind 대신 Bootboot를 사용하도록 페이지 지정 보기를 구성하려면 `AppServiceProvider` 내에서 페이지 지정자를 `UseBootstrap` 메서드를 호출하면 됩니다.

```php
// app/Providers/AppServiceProvider.php

...
use Illuminate\Pagination\Paginator;
...
public function boot()
{
    Paginator::useBootstrap();
    ...
}
```

## 네임스페이스 업데이트 라우팅

이전 Laravel 버전에서 `RouteServiceProvider`에는 컨트롤러 경로 정의에 자동으로 접두사가 붙고 작업 도우미 `URL::action` 메서드에 호출되는 `$namespace` 속성이 포함되어 있었다.

```php
// app/Providers/RouteServiceProvider.php

...

class RouteServiceProvider extends ServiceProvider
{
    protected $namespace = 'App\Http\Controllers';

    ...

}
```

이 기본값을 사용하면 다음과 같은 경로 컨트롤러를 정의할 수 있습니다.

```php
// routes/web.php
...
Route::post('login', 'UserController@login')
...
```

Laravel 8에서 `$namespace` 속성은 기본적으로 null이므로 Laravel에서 자동 네임스페이스 접두사가 수행되지 않습니다. 컨트롤러 경로 정의는 다음과 같은 표준 PHP 호출 가능 구문을 사용하여 정의해야 합니다.

```php
// routes/web.php

use App\Http\Controllers\UserController;

Route::post('/login', [UserController::class, 'login']);
```

이전 버전 스타일을 원하는 경우 `RouteServiceProvider`에서 컨트롤러 네임스페이스를 지정해야 합니다.

## 결론

이 기사에서는 라라벨 8의 새로운 기능에 대해 알아보았습니다. 현재 응용 프로그램을 버전 8로 업그레이드하려면 업그레이드 안내서와 릴리스 정보를 확인하십시오.