<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>훈발생일 뭐할래 월드컵</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
    }

    body {
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #020617;
      color: #e5e7eb;
    }

    .card {
      width: 100%;
      max-width: 480px;
      background: #020617;
      border-radius: 20px;
      padding: 24px 18px 26px;
      border: 1px solid rgba(148,163,184,0.4);
      box-shadow: 0 20px 50px rgba(0,0,0,0.6);
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 4px 10px;
      border-radius: 999px;
      font-size: 11px;
      background: rgba(15,23,42,0.9);
      border: 1px solid rgba(148,163,184,0.6);
      margin-bottom: 10px;
    }

    .badge-dot {
      width: 8px;
      height: 8px;
      border-radius: 999px;
      background: #38bdf8;
      box-shadow: 0 0 0 4px rgba(56,189,248,0.3);
    }

    .title {
      font-size: 22px;
      font-weight: 800;
      margin-bottom: 4px;
    }

    .subtitle {
      font-size: 13px;
      color: #9ca3af;
      margin-bottom: 16px;
    }

    .round-info {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      margin-bottom: 10px;
    }

    .round-text {
      font-size: 13px;
      color: #cbd5f5;
      font-weight: 600;
    }

    .match-text {
      font-size: 11px;
      color: #9ca3af;
    }

    .question {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 16px;
    }

    .options {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-bottom: 8px;
    }

    .option {
      border-radius: 16px;
      padding: 16px 14px;
      border: 1px solid rgba(148,163,184,0.5);
      background: radial-gradient(circle at top left, rgba(56,189,248,0.15), rgba(15,23,42,1));
      cursor: pointer;
      transition: transform 0.08s ease, box-shadow 0.1s ease, border 0.1s ease, background 0.15s ease;
    }

    .option:hover {
      transform: translateY(-1px);
      box-shadow: 0 14px 32px rgba(15,23,42,0.9);
      border-color: #38bdf8;
      background: radial-gradient(circle at top left, rgba(56,189,248,0.25), rgba(15,23,42,1));
    }

    .option:active {
      transform: translateY(1px) scale(0.99);
      box-shadow: 0 4px 14px rgba(15,23,42,0.9) inset;
    }

    .option-title {
      font-size: 17px;
      font-weight: 700;
      margin-bottom: 4px;
    }

    .option-desc {
      font-size: 12px;
      color: #9ca3af;
    }

    .hint {
      font-size: 11px;
      color: #6b7280;
      margin-top: 4px;
    }

    .result-title {
      font-size: 20px;
      font-weight: 700;
      margin-bottom: 8px;
    }

    .result-main {
      font-size: 17px;
      margin-bottom: 6px;
    }

    .result-sub {
      font-size: 13px;
      color: #9ca3af;
      white-space: pre-line;
      margin-bottom: 8px;
    }

    .restart {
      margin-top: 12px;
      font-size: 12px;
      color: #9ca3af;
      text-align: center;
      cursor: pointer;
    }

    .restart span {
      text-decoration: underline;
    }
  </style>
</head>
<body>

<div class="card">
  <div class="badge">
    <div class="badge-dot"></div>
    내일 뭐할래 월드컵
  </div>

  <div class="title">훈발생일인데 뭐먹을래?</div>
  <div class="subtitle" id="subtitle">둘 중 더 끌리는 걸 고르면 돼. 최종 우승이 내일 일정 🏆</div>

  <div class="round-info">
    <div class="round-text" id="round-text">8강 1경기</div>
    <div class="match-text" id="match-text"></div>
  </div>

  <div class="question">더 끌리는 걸 골라줘.</div>

  <div class="options">
    <div class="option" id="optionA">
      <div class="option-title" id="optionA-title">한식 먹기</div>
      <div class="option-desc" id="optionA-desc">예: 김치찌개, 삼겹살, 비빔밥… 든든하게 한끼.</div>
    </div>

    <div class="option" id="optionB">
      <div class="option-title" id="optionB-title">양식 먹기</div>
      <div class="option-desc" id="optionB-desc">예: 파스타, 스테이크, 피자… 분위기 있게.</div>
    </div>
  </div>

  <div class="hint" id="hint-text">후보 수: 8개 · 토너먼트 진행 중</div>

  <div class="restart" id="restart" style="display:none;">
    다시 해보고 싶다면 <span>여기를 눌러주세요</span>.
  </div>
</div>

<script>
  // ===== 1. 후보 목록 (여기만 네가 바꿔도 됨) =====
  // name: 큰 제목, desc: 설명
  const initialCandidates = [
    {
      name: "한식 먹기",
      desc: "김치찌개, 삼겹살, 비빔밥… 내일은 든든하게 한국인 소울푸드."
    },
    {
      name: "양식 먹기",
      desc: "파스타, 스테이크, 피자… 살짝 특별한 분위기 내기."
    },
    {
      name: "일식 먹기",
      desc: "초밥, 라멘, 돈까스… 깔끔하게 한 끼."
    },
    {
      name: "가성비 오마카세",
      desc: "추운날엔 회가 신선하지."
    },
    {
      name: "와인바",
      desc: "해방촌같은곳에서 스몰플레이트랑 와인한잔."
    },
    {
      name: "집에서 배달",
      desc: "밖은 시끄럽고 복잡해…?"
    },
    {
      name: "고기먹으러갈까",
      desc: "삼겹살이나 소고기"
    },
    {
      name: "시장데이트",
      desc: "시장구경도 좋아."
    }
  ];

  // ===== 2. 상태 변수 =====
  let currentRound = [];
  let nextRound = [];
  let matchIndex = 0;
  let roundNumber = 1;

  // ===== 3. DOM 요소 =====
  const roundText = document.getElementById("round-text");
  const matchText = document.getElementById("match-text");
  const hintText = document.getElementById("hint-text");
  const subtitle = document.getElementById("subtitle");

  const optionA = document.getElementById("optionA");
  const optionB = document.getElementById("optionB");
  const optionATitle = document.getElementById("optionA-title");
  const optionADesc = document.getElementById("optionA-desc");
  const optionBTitle = document.getElementById("optionB-title");
  const optionBDesc = document.getElementById("optionB-desc");

  const restartEl = document.getElementById("restart");

  // ===== 4. 유틸: 배열 셔플 =====
  function shuffle(array) {
    const arr = array.slice();
    for (let i = arr.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr;
  }

  // ===== 5. 라운드 이름 생성 =====
  function getRoundName(size) {
    if (size === 2) return "결승";
    if (size === 4) return "4강";
    if (size === 8) return "8강";
    if (size === 16) return "16강";
    return `${size}강`;
  }

  // ===== 6. 경기 화면 렌더링 =====
  function renderMatch() {
    // 라운드가 끝났는지 먼저 체크
    const totalPairs = Math.floor(currentRound.length / 2);
    if (matchIndex >= totalPairs) {
      // 홀수일 경우 자동진출 1명 처리
      if (currentRound.length % 2 === 1) {
        nextRound.push(currentRound[currentRound.length - 1]);
      }

      // 다음 라운드로 진행
      if (nextRound.length === 1) {
        // 우승 확정
        renderWinner(nextRound[0]);
        return;
      } else {
        currentRound = nextRound.slice();
        nextRound = [];
        matchIndex = 0;
        roundNumber++;
      }
    }

    const size = currentRound.length;
    const roundName = getRoundName(size);
    const currentMatchNumber = matchIndex + 1;
    const totalMatches = Math.floor(size / 2);

    roundText.textContent = `${roundName} ${currentMatchNumber}경기`;
    matchText.textContent = `이번 라운드 후보 수: ${size}개 · 총 ${totalMatches}경기 중 ${currentMatchNumber}경기`;

    const indexA = matchIndex * 2;
    const indexB = matchIndex * 2 + 1;
    const candidateA = currentRound[indexA];
    const candidateB = currentRound[indexB];

    optionATitle.textContent = candidateA.name;
    optionADesc.textContent = candidateA.desc || "";
    optionBTitle.textContent = candidateB.name;
    optionBDesc.textContent = candidateB.desc || "";

    hintText.textContent = "둘 중 더 끌리는 내일의 계획을 골라줘.";
  }

  // ===== 7. 우승 결과 렌더링 =====
  function renderWinner(winner) {
    const roundName = "최종 결과";

    roundText.textContent = roundName;
    matchText.textContent = "";

    const winnerTitle = `내일은 이게 딱이다 🏆`;
    const main = winner.name;
    const sub =
      (winner.desc || "") +
      "\n\n이걸 최종으로 고른 당신,\n이미 마음은 내일 일정에 가 있는 중입니다. 😉";

    optionA.style.display = "none";
    optionB.style.display = "none";

    const card = document.querySelector(".card");
    const questionEl = document.querySelector(".question");
    questionEl.textContent = winnerTitle;

    // 결과 영역을 임시로 optionA 자리 활용해서 띄우는 대신,
    // hintText에 결과 텍스트를 넣는 방식도 가능하지만
    // 여기서는 아래처럼 깔끔하게 교체
    const resultHtml = `
      <div class="result-title">${winnerTitle}</div>
      <div class="result-main">${main}</div>
      <div class="result-sub">${sub}</div>
    `;
    hintText.innerHTML = resultHtml;
    subtitle.textContent = "다시 하고 싶으면 아래를 눌러서 리셋할 수 있어요.";

    restartEl.style.display = "block";
  }

  // ===== 8. 선택 처리 =====
  function choose(side) {
    const indexA = matchIndex * 2;
    const indexB = matchIndex * 2 + 1;
    const candidateA = currentRound[indexA];
    const candidateB = currentRound[indexB];

    if (side === "A") {
      nextRound.push(candidateA);
    } else {
      nextRound.push(candidateB);
    }

    matchIndex++;
    renderMatch();
  }

  optionA.addEventListener("click", () => choose("A"));
  optionB.addEventListener("click", () => choose("B"));

  // ===== 9. 다시 시작 =====
  function restart() {
    currentRound = shuffle(initialCandidates);
    nextRound = [];
    matchIndex = 0;
    roundNumber = 1;

    // UI 초기화
    optionA.style.display = "block";
    optionB.style.display = "block";
    hintText.textContent = "후보 수: " + currentRound.length + "개 · 토너먼트 진행 중";
    subtitle.textContent = "둘 중 더 끌리는 걸 고르면 돼. 최종 우승이 내일 일정 🏆";
    restartEl.style.display = "none";

    renderMatch();
  }

  restartEl.addEventListener("click", restart);

  // ===== 10. 첫 시작 =====
  function init() {
    currentRound = shuffle(initialCandidates);
    nextRound = [];
    matchIndex = 0;
    roundNumber = 1;
    renderMatch();
  }

  init();
</script>

</body>
</html>
