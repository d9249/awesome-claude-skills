---
name: slack-gif-creator
description: 크기 제약 조건에 대한 검증기와 구성 가능한 애니메이션 프리미티브가 있는 Slack에 최적화된 애니메이션 GIF를 생성하는 툴킷입니다. 이 스킬은 사용자가 "X가 Y를 하는 Slack용 GIF를 만들어주세요"와 같은 설명에서 Slack용 애니메이션 GIF 또는 이모지 애니메이션을 요청할 때 적용됩니다.
license: 전체 조건은 LICENSE.txt 참조
---

# Slack GIF 생성기 - 유연한 툴킷

Slack에 최적화된 애니메이션 GIF를 생성하는 툴킷입니다. Slack의 제약 조건을 위한 검증기, 구성 가능한 애니메이션 프리미티브 및 선택적 도우미 유틸리티를 제공합니다. **창의적 비전을 달성하기 위해 필요에 따라 이러한 도구를 적용하세요.**

## Slack의 요구사항

Slack은 사용에 따라 GIF에 대한 특정 요구사항이 있습니다:

**메시지 GIF:**
- 최대 크기: ~2MB
- 최적 크기: 480x480
- 일반적인 FPS: 15-20
- 색상 제한: 128-256
- 시간: 2-5초

**이모지 GIF:**
- 최대 크기: 64KB (엄격한 제한)
- 최적 크기: 128x128
- 일반적인 FPS: 10-12
- 색상 제한: 32-48
- 시간: 1-2초

**이모지 GIF는 까다롭습니다** - 64KB 제한은 엄격합니다. 도움이 되는 전략:
- 총 10-15 프레임으로 제한
- 최대 32-48색 사용
- 디자인을 단순하게 유지
- 그라데이션 피하기
- 파일 크기를 자주 검증

## 툴킷 구조

이 스킬은 세 가지 유형의 도구를 제공합니다:

1. **검증기** - GIF가 Slack의 요구사항을 충족하는지 확인
2. **애니메이션 프리미티브** - 모션을 위한 구성 가능한 빌딩 블록 (shake, bounce, move, kaleidoscope)
3. **도우미 유틸리티** - 일반적인 요구사항을 위한 선택적 함수 (텍스트, 색상, 효과)

**이러한 도구를 적용하는 방법에 대한 완전한 창의적 자유가 있습니다.**

## 핵심 검증기

GIF가 Slack의 제약 조건을 충족하는지 확인하려면 다음 검증기를 사용하세요:

```python
from core.gif_builder import GIFBuilder

# GIF를 만든 후 요구사항을 충족하는지 확인
builder = GIFBuilder(width=128, height=128, fps=10)
# ... 원하는 대로 프레임 추가 ...

# 저장하고 크기 확인
info = builder.save('emoji.gif', num_colors=48, optimize_for_emoji=True)

# save 메서드는 파일이 제한을 초과하면 자동으로 경고
# info dict 포함: size_kb, size_mb, frame_count, duration_seconds
```

**파일 크기 검증기**:
```python
from core.validators import check_slack_size

# GIF가 크기 제한을 충족하는지 확인
passes, info = check_slack_size('emoji.gif', is_emoji=True)
# 반환: (True/False, 크기 세부 정보가 있는 dict)
```

**크기 검증기**:
```python
from core.validators import validate_dimensions

# 크기 확인
passes, info = validate_dimensions(128, 128, is_emoji=True)
# 반환: (True/False, 크기 세부 정보가 있는 dict)
```

**완전한 검증**:
```python
from core.validators import validate_gif, is_slack_ready

# 모든 검증 실행
all_pass, results = validate_gif('emoji.gif', is_emoji=True)

# 또는 빠른 확인
if is_slack_ready('emoji.gif', is_emoji=True):
    print("업로드 준비 완료!")
```

## 애니메이션 프리미티브

이것들은 모션을 위한 구성 가능한 빌딩 블록입니다. 이것들을 어떤 조합으로든 어떤 객체에든 적용하세요:

### 흔들기
```python
from templates.shake import create_shake_animation

# 이모지 흔들기
frames = create_shake_animation(
    object_type='emoji',
    object_data={'emoji': '😱', 'size': 80},
    num_frames=20,
    shake_intensity=15,
    direction='both'  # 또는 'horizontal', 'vertical'
)
```

### 튕기기
```python
from templates.bounce import create_bounce_animation

# 원 튕기기
frames = create_bounce_animation(
    object_type='circle',
    object_data={'radius': 40, 'color': (255, 100, 100)},
    num_frames=30,
    bounce_height=150
)
```

### 회전
```python
from templates.spin import create_spin_animation, create_loading_spinner

# 시계 방향 회전
frames = create_spin_animation(
    object_type='emoji',
    object_data={'emoji': '🔄', 'size': 100},
    rotation_type='clockwise',
    full_rotations=2
)

# 흔들림 회전
frames = create_spin_animation(rotation_type='wobble', full_rotations=3)

# 로딩 스피너
frames = create_loading_spinner(spinner_type='dots')
```

### 펄스 / 심장 박동
```python
from templates.pulse import create_pulse_animation, create_attention_pulse

# 부드러운 펄스
frames = create_pulse_animation(
    object_data={'emoji': '❤️', 'size': 100},
    pulse_type='smooth',
    scale_range=(0.8, 1.2)
)

# 심장 박동 (이중 펌프)
frames = create_pulse_animation(pulse_type='heartbeat')

# 이모지 GIF를 위한 주의 펄스
frames = create_attention_pulse(emoji='⚠️', num_frames=20)
```

### 페이드
```python
from templates.fade import create_fade_animation, create_crossfade

# 페이드 인
frames = create_fade_animation(fade_type='in')

# 페이드 아웃
frames = create_fade_animation(fade_type='out')

# 두 이모지 간 크로스페이드
frames = create_crossfade(
    object1_data={'emoji': '😊', 'size': 100},
    object2_data={'emoji': '😂', 'size': 100}
)
```

### 줌
```python
from templates.zoom import create_zoom_animation, create_explosion_zoom

# 극적으로 줌 인
frames = create_zoom_animation(
    zoom_type='in',
    scale_range=(0.1, 2.0),
    add_motion_blur=True
)

# 줌 아웃
frames = create_zoom_animation(zoom_type='out')

# 폭발 줌
frames = create_explosion_zoom(emoji='💥')
```

### 폭발 / 산산이 부서지기
```python
from templates.explode import create_explode_animation, create_particle_burst

# 버스트 폭발
frames = create_explode_animation(
    explode_type='burst',
    num_pieces=25
)

# 산산이 부서지는 효과
frames = create_explode_animation(explode_type='shatter')

# 입자로 녹아내림
frames = create_explode_animation(explode_type='dissolve')

# 입자 버스트
frames = create_particle_burst(particle_count=30)
```

### 흔들림 / 지글거림
```python
from templates.wiggle import create_wiggle_animation, create_excited_wiggle

# 젤로 흔들림
frames = create_wiggle_animation(
    wiggle_type='jello',
    intensity=1.0,
    cycles=2
)

# 파동 동작
frames = create_wiggle_animation(wiggle_type='wave')

# 이모지 GIF를 위한 흥분된 흔들림
frames = create_excited_wiggle(emoji='🎉')
```

### 슬라이드
```python
from templates.slide import create_slide_animation, create_multi_slide

# 왼쪽에서 오버슈트와 함께 슬라이드 인
frames = create_slide_animation(
    direction='left',
    slide_type='in',
    overshoot=True
)

# 가로질러 슬라이드
frames = create_slide_animation(direction='left', slide_type='across')

# 순차적으로 슬라이드하는 여러 객체
objects = [
    {'data': {'emoji': '🎯', 'size': 60}, 'direction': 'left', 'final_pos': (120, 240)},
    {'data': {'emoji': '🎪', 'size': 60}, 'direction': 'right', 'final_pos': (240, 240)}
]
frames = create_multi_slide(objects, stagger_delay=5)
```

### 뒤집기
```python
from templates.flip import create_flip_animation, create_quick_flip

# 두 이모지 간 수평 뒤집기
frames = create_flip_animation(
    object1_data={'emoji': '😊', 'size': 120},
    object2_data={'emoji': '😂', 'size': 120},
    flip_axis='horizontal'
)

# 수직 뒤집기
frames = create_flip_animation(flip_axis='vertical')

# 이모지 GIF를 위한 빠른 뒤집기
frames = create_quick_flip('👍', '👎')
```

### 변형 / 변환
```python
from templates.morph import create_morph_animation, create_reaction_morph

# 크로스페이드 변형
frames = create_morph_animation(
    object1_data={'emoji': '😊', 'size': 100},
    object2_data={'emoji': '😂', 'size': 100},
    morph_type='crossfade'
)

# 스케일 변형 (하나는 축소하고 다른 하나는 성장)
frames = create_morph_animation(morph_type='scale')

# 스핀 변형 (3D 뒤집기 같은)
frames = create_morph_animation(morph_type='spin_morph')
```

### 이동 효과
```python
from templates.move import create_move_animation

# 선형 이동
frames = create_move_animation(
    object_type='emoji',
    object_data={'emoji': '🚀', 'size': 60},
    start_pos=(50, 240),
    end_pos=(430, 240),
    motion_type='linear',
    easing='ease_out'
)

# 호 이동 (포물선 궤적)
frames = create_move_animation(
    object_type='emoji',
    object_data={'emoji': '⚽', 'size': 60},
    start_pos=(50, 350),
    end_pos=(430, 350),
    motion_type='arc',
    motion_params={'arc_height': 150}
)

# 원형 이동
frames = create_move_animation(
    object_type='emoji',
    object_data={'emoji': '🌍', 'size': 50},
    motion_type='circle',
    motion_params={
        'center': (240, 240),
        'radius': 120,
        'angle_range': 360  # 전체 원
    }
)

# 파동 이동
frames = create_move_animation(
    motion_type='wave',
    motion_params={
        'wave_amplitude': 50,
        'wave_frequency': 2
    }
)

# 또는 저수준 이징 함수 사용
from core.easing import interpolate, calculate_arc_motion

for i in range(num_frames):
    t = i / (num_frames - 1)
    x = interpolate(start_x, end_x, t, easing='ease_out')
    # 또는: x, y = calculate_arc_motion(start, end, height, t)
```

### 만화경 효과
```python
from templates.kaleidoscope import apply_kaleidoscope, create_kaleidoscope_animation

# 단일 프레임에 적용
kaleido_frame = apply_kaleidoscope(frame, segments=8)

# 또는 애니메이션 만화경 생성
frames = create_kaleidoscope_animation(
    base_frame=my_frame,  # 또는 데모 패턴을 위한 None
    num_frames=30,
    segments=8,
    rotation_speed=1.0
)

# 간단한 미러 효과 (더 빠름)
from templates.kaleidoscope import apply_simple_mirror

mirrored = apply_simple_mirror(frame, mode='quad')  # 4방향 미러
# 모드: 'horizontal', 'vertical', 'quad', 'radial'
```

**프리미티브를 자유롭게 구성하려면 다음 패턴을 따르세요:**
```python
# 예시: 충격을 위한 튕기기 + 흔들기
for i in range(num_frames):
    frame = create_blank_frame(480, 480, bg_color)

    # 튕기기 동작
    t_bounce = i / (num_frames - 1)
    y = interpolate(start_y, ground_y, t_bounce, 'bounce_out')

    # 충격 시 흔들기 추가 (y가 지면에 도달할 때)
    if y >= ground_y - 5:
        shake_x = math.sin(i * 2) * 10
        x = center_x + shake_x
    else:
        x = center_x

    draw_emoji(frame, '⚽', (x, y), size=60)
    builder.add_frame(frame)
```

## 도우미 유틸리티

이것들은 일반적인 요구사항을 위한 선택적 도우미입니다. **필요에 따라 사용하거나 수정하거나 사용자 정의 구현으로 교체하세요.**

### GIF 빌더 (조립 및 최적화)

```python
from core.gif_builder import GIFBuilder

# 선택한 설정으로 빌더 생성
builder = GIFBuilder(width=480, height=480, fps=20)

# 프레임 추가 (어떻게 만들었든)
for frame in my_frames:
    builder.add_frame(frame)

# 최적화와 함께 저장
builder.save('output.gif',
             num_colors=128,
             optimize_for_emoji=False)
```

주요 기능:
- 자동 색상 양자화
- 중복 프레임 제거
- Slack 제한에 대한 크기 경고
- 이모지 모드 (적극적인 최적화)

### 텍스트 렌더링

이모지와 같은 작은 GIF의 경우 텍스트 가독성이 어렵습니다. 일반적인 솔루션은 윤곽선을 추가하는 것입니다:

```python
from core.typography import draw_text_with_outline, TYPOGRAPHY_SCALE

# 윤곽선이 있는 텍스트 (가독성 향상)
draw_text_with_outline(
    frame, "BONK!",
    position=(240, 100),
    font_size=TYPOGRAPHY_SCALE['h1'],  # 60px
    text_color=(255, 68, 68),
    outline_color=(0, 0, 0),
    outline_width=4,
    centered=True
)
```

사용자 정의 텍스트 렌더링을 구현하려면 큰 GIF에 잘 작동하는 PIL의 `ImageDraw.text()`를 사용하세요.

### 색상 관리

전문적으로 보이는 GIF는 종종 응집력 있는 색상 팔레트를 사용합니다:

```python
from core.color_palettes import get_palette

# 미리 만들어진 팔레트 가져오기
palette = get_palette('vibrant')  # 또는 'pastel', 'dark', 'neon', 'professional'

bg_color = palette['background']
text_color = palette['primary']
accent_color = palette['accent']
```

색상을 직접 작업하려면 RGB 튜플을 사용하세요 - 사용 사례에 맞는 것이면 됩니다.

### 시각 효과

충격 순간을 위한 선택적 효과:

```python
from core.visual_effects import ParticleSystem, create_impact_flash, create_shockwave_rings

# 입자 시스템
particles = ParticleSystem()
particles.emit_sparkles(x=240, y=200, count=15)
particles.emit_confetti(x=240, y=200, count=20)

# 각 프레임 업데이트 및 렌더링
particles.update()
particles.render(frame)

# 플래시 효과
frame = create_impact_flash(frame, position=(240, 200), radius=100)

# 충격파 링
frame = create_shockwave_rings(frame, position=(240, 200), radii=[30, 60, 90])
```

### 이징 함수

부드러운 동작은 선형 보간 대신 이징을 사용합니다:

```python
from core.easing import interpolate

# 떨어지는 객체 (가속)
y = interpolate(start=0, end=400, t=progress, easing='ease_in')

# 착지하는 객체 (감속)
y = interpolate(start=0, end=400, t=progress, easing='ease_out')

# 튕기기
y = interpolate(start=0, end=400, t=progress, easing='bounce_out')

# 오버슈트 (탄성)
scale = interpolate(start=0.5, end=1.0, t=progress, easing='elastic_out')
```

사용 가능한 이징: `linear`, `ease_in`, `ease_out`, `ease_in_out`, `bounce_out`, `elastic_out`, `back_out` (오버슈트), 그리고 `core/easing.py`에 더 많이 있습니다.

### 프레임 구성

필요한 경우 기본 그리기 유틸리티:

```python
from core.frame_composer import (
    create_gradient_background,  # 그라데이션 배경
    draw_emoji_enhanced,         # 선택적 그림자가 있는 이모지
    draw_circle_with_shadow,     # 깊이가 있는 도형
    draw_star                    # 5개 끝 별
)

# 그라데이션 배경
frame = create_gradient_background(480, 480, top_color, bottom_color)

# 그림자가 있는 이모지
draw_emoji_enhanced(frame, '🎉', position=(200, 200), size=80, shadow=True)
```

## 최적화 전략

GIF가 너무 큰 경우:

**메시지 GIF (>2MB):**
1. 프레임 감소 (FPS 낮추기 또는 시간 단축)
2. 색상 감소 (128 → 64색)
3. 크기 감소 (480x480 → 320x320)
4. 중복 프레임 제거 활성화

**이모지 GIF (>64KB) - 적극적으로:**
1. 총 10-12 프레임으로 제한
2. 최대 32-40색 사용
3. 그라데이션 피하기 (단색이 더 잘 압축됨)
4. 디자인 단순화 (더 적은 요소)
5. save 메서드에서 `optimize_for_emoji=True` 사용

## 예시 구성 패턴

### 간단한 반응 (펄싱)
```python
builder = GIFBuilder(128, 128, 10)

for i in range(12):
    frame = Image.new('RGB', (128, 128), (240, 248, 255))

    # 펄싱 스케일
    scale = 1.0 + math.sin(i * 0.5) * 0.15
    size = int(60 * scale)

    draw_emoji_enhanced(frame, '😱', position=(64-size//2, 64-size//2),
                       size=size, shadow=False)
    builder.add_frame(frame)

builder.save('reaction.gif', num_colors=40, optimize_for_emoji=True)

# 검증
from core.validators import check_slack_size
check_slack_size('reaction.gif', is_emoji=True)
```

### 충격이 있는 액션 (튕기기 + 플래시)
```python
builder = GIFBuilder(480, 480, 20)

# 1단계: 객체가 떨어짐
for i in range(15):
    frame = create_gradient_background(480, 480, (240, 248, 255), (200, 230, 255))
    t = i / 14
    y = interpolate(0, 350, t, 'ease_in')
    draw_emoji_enhanced(frame, '⚽', position=(220, int(y)), size=80)
    builder.add_frame(frame)

# 2단계: 충격 + 플래시
for i in range(8):
    frame = create_gradient_background(480, 480, (240, 248, 255), (200, 230, 255))

    # 첫 번째 프레임에 플래시
    if i < 3:
        frame = create_impact_flash(frame, (240, 350), radius=120, intensity=0.6)

    draw_emoji_enhanced(frame, '⚽', position=(220, 350), size=80)

    # 텍스트 나타남
    if i > 2:
        draw_text_with_outline(frame, "GOAL!", position=(240, 150),
                              font_size=60, text_color=(255, 68, 68),
                              outline_color=(0, 0, 0), outline_width=4, centered=True)

    builder.add_frame(frame)

builder.save('goal.gif', num_colors=128)
```

### 프리미티브 결합 (이동 + 흔들기)
```python
from templates.shake import create_shake_animation

# 흔들기 애니메이션 생성
shake_frames = create_shake_animation(
    object_type='emoji',
    object_data={'emoji': '😰', 'size': 70},
    num_frames=20,
    shake_intensity=12
)

# 흔들기를 트리거하는 이동 요소 생성
builder = GIFBuilder(480, 480, 20)
for i in range(40):
    t = i / 39

    if i < 20:
        # 트리거 전 - 이동하는 객체가 있는 빈 프레임 사용
        frame = create_blank_frame(480, 480, (255, 255, 255))
        x = interpolate(50, 300, t * 2, 'linear')
        draw_emoji_enhanced(frame, '🚗', position=(int(x), 300), size=60)
        draw_emoji_enhanced(frame, '😰', position=(350, 200), size=70)
    else:
        # 트리거 후 - 흔들기 프레임 사용
        frame = shake_frames[i - 20]
        # 최종 위치에 차 추가
        draw_emoji_enhanced(frame, '🚗', position=(300, 300), size=60)

    builder.add_frame(frame)

builder.save('scare.gif')
```

## 철학

이 툴킷은 엄격한 레시피가 아닌 빌딩 블록을 제공합니다. GIF 요청을 작업하려면:

1. **창의적 비전 이해** - 무슨 일이 일어나야 하나요? 분위기는 무엇인가요?
2. **애니메이션 디자인** - 단계로 나누기 (예상, 액션, 반응)
3. **필요에 따라 프리미티브 적용** - 흔들기, 튕기기, 이동, 효과 - 자유롭게 믹스
4. **제약 조건 검증** - 파일 크기 확인, 특히 이모지 GIF의 경우
5. **필요시 반복** - 크기 제한을 초과하면 프레임/색상 감소

**목표는 Slack의 기술적 제약 조건 내에서 창의적 자유입니다.**

## 종속성

이 툴킷을 사용하려면 아직 없는 경우에만 다음 종속성을 설치하세요:

```bash
pip install pillow imageio numpy
```
