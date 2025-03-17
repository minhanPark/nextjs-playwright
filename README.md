# nextjs playwrite

playwright는 e2e 테스트의 다른 옵션이다.

## 설치

```bash
npm init playwright
```

위의 명령어를 실행하면 몇가지 프롬프트를 고를 수 있게 그에 맞게 playwright를 설치해준다. 그리고 playwright.config.ts도 생성해준다.

```bash
# 강의처럼 아래와 같이 입력했다.

√ Where to put your end-to-end tests? · src/e2e
√ Add a GitHub Actions workflow? (y/N) · false
√ Install Playwright browsers (can be done manually via 'npx playwright install')? (Y/n) · true
```

src 디렉토리를 사용했기 때문에 e2e 폴더로 만들었고, github actions를 사용하지 않았기 때문에 false로 했다.

```ts
// playwright.config.ts

export default defineConfig({
  use: {
    /* Base URL to use in actions like `await page.goto('/')`. */
    // baseURL: 'http://127.0.0.1:3000',
    baseURL: "http://127.0.0.1:3000",
    /* Collect trace when retrying the failed test. See https://playwright.dev/docs/trace-viewer */
    trace: "on-first-retry",
  },
  /* Run your local dev server before starting the tests */
  // webServer: {
  //   command: 'npm run start',
  //   url: 'http://127.0.0.1:3000',
  //   reuseExistingServer: !process.env.CI,
  // },
  webServer: {
    command: "npm run dev",
    url: "http://127.0.0.1:3000",
    reuseExistingServer: !process.env.CI,
  },
});
```

playwright.config.ts 파일을 설정해준다. baseUrl을 설정해주면 테스트 시 전체 url을 적지 않고 "/" 등으로 적을 수 있다.  
webServe에 커맨드도 수정해서 dev 서버에 맞게 설정해준다.

## 테스트 작성

```ts
// src/e2e/home.spec.ts

import { test, expect } from "@playwright/test";

test("simple home page test", async ({ page }) => {
  await page.goto("/");

  await expect(page.getByText("bulbasaur")).toBeVisible();
});
```

위와 같이 파일을 작성하고 playwright를 실행하면 된다.

```bash
npx playwright test

# npx playwright show-report
# 위의 명령어로 html로 된 리포트를 볼 수도 있다.
```
