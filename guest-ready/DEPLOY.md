# Guest Ready — Netlify Deploy

Target URL: `https://guest-ready.netlify.app/`

## GitHub source
- Repository: `HotCharliepepper/WeplaceIntroduction`
- Production branch: `guest-ready`
- Base directory: `guest-ready`
- Build command: none
- Publish directory: `.`

## Netlify setup
1. Add new project → Import an existing project → GitHub.
2. Select `HotCharliepepper/WeplaceIntroduction`.
3. Set production branch to `guest-ready`.
4. Set base directory to `guest-ready`.
5. Leave build command empty.
6. Set publish directory to `.`.
7. Deploy.
8. Site configuration → Change site name → `guest-ready`.

## Post-deploy checks
- `/` loads the landing page.
- `/og/guest-ready-og.jpg` opens successfully.
- Link preview title: `외국인 손님은 우리 매장을 어떻게 보고 있을까요?`
- Link preview subtitle: `Google · Maps · Photos · Reviews · Booking`
- 30-second Guest Ready Check works on mobile.
