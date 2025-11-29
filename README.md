<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎄 야식 밸런스 게임: 나의 야식 MBTI는?</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* 커스텀 스타일 및 애니메이션 정의 */
        @keyframes bounce {
            0%, 100% { transform: translateY(-5%); }
            50% { transform: translateY(0); }
        }
        .animate-bounce-slow {
            animation: bounce 3s infinite ease-in-out;
        }

        /* 크리스마스 테마 색상 설정 */
        :root {
            --color-red: #ef4444; /* tailwind red-500 */
            --color-green: #10b981; /* tailwind emerald-500 */
            --color-gold: #f59e0b; /* tailwind amber-500 */
        }

        .bg-xmas-red { background-color: var(--color-red); }
        .text-xmas-green { color: var(--color-green); }
        .border-xmas-gold { border-color: var(--color-gold); }

        /* 카드 선택 시 강조 스타일 */
        .food-card:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .food-card {
            transition: all 0.3s ease-in-out;
        }
    </style>
    <script>
        // Tailwind CSS 커스터마이징 설정 (선택 사항)
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'xmas-red': '#ef4444',
                        'xmas-green': '#10b981',
                        'xmas-gold': '#f59e0b',
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-gray-50 min-h-screen p-4 flex flex-col items-center">

    <div id="app" class="w-full max-w-4xl bg-white shadow-2xl rounded-3xl p-6 md:p-10 border-8 border-xmas-red">
        
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-extrabold text-xmas-green mb-2 flex items-center justify-center">
                <span class="text-5xl md:text-6xl animate-bounce-slow">🎅</span>
                <span class="mx-3">야식 밸런스 게임</span>
                <span class="text-5xl md:text-6xl animate-bounce-slow">🎁</span>
            </h1>
            <p class="text-xl text-gray-600 font-semibold">"나의 야식 MBTI"를 찾아봐!</p>
        </header>

        <section id="nickname-screen" class="space-y-6">
            <h2 class="text-3xl font-bold text-center text-xmas-red">✨ 시작하기 전에</h2>
            <div class="flex flex-col items-center space-y-4">
                <p class="text-lg text-gray-700">함께 게임할 당신의 **귀여운 별명**을 입력해주세요!</p>
                <input type="text" id="nickname-input" placeholder="예: 짱구, 폼폼푸린" 
                       class="p-3 border-4 border-xmas-green rounded-xl w-full max-w-sm text-center text-xl focus:outline-none focus:ring-4 focus:ring-xmas-gold">
                <button onclick="startGame()" class="bg-xmas-green text-white font-bold py-3 px-8 rounded-full text-xl shadow-lg hover:bg-green-700 transition duration-300 transform hover:scale-105">
                    게임 시작!
                </button>
            </div>
        </section>

        <section id="game-screen" class="hidden space-y-8">
            <h2 id="current-question" class="text-3xl font-bold text-center text-gray-800"></h2>
            
            <div class="flex flex-col md:flex-row justify-center items-center space-y-6 md:space-y-0 md:space-x-8">
                
                <div id="option-a" class="food-card flex flex-col items-center bg-xmas-green text-white p-6 rounded-2xl w-full md:w-56 cursor-pointer border-4 border-xmas-gold shadow-xl" onclick="selectOption(0)">
                    <img id="image-a" src="" alt="야식 A" class="w-24 h-24 object-cover rounded-full mb-3 border-2 border-white animate-bounce-slow">
                    <p id="menu-a" class="text-2xl font-extrabold text-center"></p>
                    <span class="mt-2 text-sm">👈 이걸 선택할래요!</span>
                </div>
                
                <div class="text-5xl font-black text-xmas-red px-4 py-2 bg-white rounded-full border-4 border-xmas-gold transform rotate-0 md:rotate-90">
                    VS.
                </div>

                <div id="option-b" class="food-card flex flex-col items-center bg-xmas-red text-white p-6 rounded-2xl w-full md:w-56 cursor-pointer border-4 border-xmas-gold shadow-xl" onclick="selectOption(1)">
                    <img id="image-b" src="" alt="야식 B" class="w-24 h-24 object-cover rounded-full mb-3 border-2 border-white animate-bounce-slow">
                    <p id="menu-b" class="text-2xl font-extrabold text-center"></p>
                    <span class="mt-2 text-sm">이걸 선택할래요! 👉</span>
                </div>
            </div>

            <p class="text-center text-gray-500 text-sm mt-4">📢 총 <span id="total-questions"></span>문제 중 <span id="current-index"></span>번째 문제!</p>
        </section>

        <section id="result-screen" class="hidden text-center space-y-8">
            <h2 class="text-4xl font-extrabold text-xmas-red">🎉 결과 발표!</h2>
            
            <div class="bg-yellow-100 p-8 rounded-3xl border-4 border-xmas-gold shadow-2xl space-y-4 transform scale-95 transition duration-500 ease-out hover:scale-100">
                <p id="result-nickname" class="text-2xl text-xmas-green font-bold"></p>
                <p class="text-5xl font-black text-xmas-red flex items-center justify-center">
                    <span id="final-mbti"></span>
                </p>
                <div class="text-xl text-gray-700">
                    <p id="result-food-name" class="font-bold"></p>
                    <p id="result-food-desc"></p>
                </div>
            </div>

            <div id="sanrio-character" class="flex flex-col items-center p-4 bg-pink-50 rounded-2xl border-2 border-pink-300">
                <p class="text-2xl font-bold text-pink-600 mb-3">💖 나의 소울메이트 캐릭터는?</p>
                <img id="sanrio-image" src="" alt="산리오 캐릭터" class="w-32 h-32 md:w-40 md:h-40 object-contain mb-3 animate-pulse">
                <p id="sanrio-name" class="text-xl font-extrabold text-pink-700"></p>
                <p id="sanrio-desc" class="text-gray-600 text-sm"></p>
            </div>
            
            <button onclick="location.reload()" class="bg-xmas-green text-white font-bold py-3 px-8 rounded-full text-xl shadow-lg hover:bg-green-700 transition duration-300 transform hover:scale-105 mt-6">
                🔄 다시 시작! (친구와 함께!)
            </button>
        </section>

    </div>
    
    <script>
        // 1. 야식 밸런스 게임 데이터 정의
        const balanceGameData = [
            { question: "매콤한 것이 땡기는 날 🔥",
              options: [{ name: "엽기 떡볶이", type: "E", img: "🌶️" }, { name: "교촌 허니콤보", type: "I", img: "🍗" }] },
            { question: "뜨끈한 국물이 필요할 때 🍜",
              options: [{ name: "어묵탕 & 소주", type: "S", img: "🍢" }, { name: "라면 & 김치", type: "N", img: "🍜" }] },
            { question: "달달한 행복을 찾는다면 🍦",
              options: [{ name: "붕어빵 팥/슈크림", type: "T", img: "붕" }, { name: "아이스크림 케이크", type: "F", img: "🎂" }] },
            { question: "가장 만족스러운 마무리 🍚",
              options: [{ name: "편의점 꿀조합 (정석)", type: "J", img: "🏪" }, { name: "배달 앱 켜서 서칭 (모험)", type: "P", img: "🛵" }] },
        ];

        // 2. MBTI와 최종 야식 & 산리오 캐릭터 연결 데이터
        // 결과는 각 질문의 첫 번째 옵션(type) 선택 횟수를 기준으로 계산됨. (E/S/T/J)
        const resultMappings = {
            "ESTJ": { food: "국룰 치킨 (후라이드 반 양념 반)", desc: "계획적이고 전통을 중시하는 당신에게 국룰 조합은 불변의 진리! 실패는 없다.", sanrio: { name: "헬로키티", img: "images/kitty.png", desc: "모두에게 사랑받는 만능 엔터테이너! 🎀" } },
            "ENFP": { food: "마라탕 & 꿔바로우", desc: "자유롭고 창의적인 당신은 새로운 맛에 도전하는 것을 즐긴다. 복잡한 마라탕 재료는 당신의 다양한 아이디어와 닮았다.", sanrio: { name: "마이멜로디", img: "images/mymelo.png", desc: "밝고 명랑한 매력! 🌸" } },
            "ISTP": { food: "감자튀김과 맥주", desc: "과묵하고 실용적인 당신에게 복잡한 요리는 불필요하다. 심플하고 확실한 만족감을 주는 조합을 선호한다.", sanrio: { name: "쿠로미", img: "images/kuromi.png", desc: "시크하고 쿨한 독특한 매력! 🖤" } },
            "ISFJ": { food: "잔치국수와 김치전", desc: "따뜻하고 배려심 깊은 당신은 엄마의 손맛 같은 정겹고 푸근한 야식을 선택한다. 남을 잘 챙긴다.", sanrio: { name: "시나모롤", img: "images/cinna.png", desc: "천사같이 착하고 다정한 친구! ☁️" } },
            "INTP": { food: "컵라면과 삼각김밥", desc: "논리적이고 분석적인 당신은 가성비와 효율성을 추구한다. 이성적인 선택의 끝판왕 야식.", sanrio: { name: "폼폼푸린", img: "images/purin.png", desc: "느긋하고 여유로운 귀여움! 🍮" } },
            "ELSE": { food: "떡볶이 & 순대 & 튀김", desc: "당신은 한국인의 소울푸드를 사랑하는 진정한 미식가! (계산 오류일 수도 있어요🤣)", sanrio: { name: "리틀 트윈스타 (키키&라라)", img: "images/kiki-lala.png", desc: "신비롭고 꿈결 같은 환상의 조합! 💫" } },
        };
        
        // **[참고]** 산리오 이미지 (CDN 링크 사용 - 실제 배포 시에는 직접 호스팅 권장)
        // 안전한 플레이스를 위해 임의의 PNG 이미지 URL을 사용합니다.
        const sanrioImages = {
            "헬로키티": "https://i.imgur.com/Gj3H9fL.png", // Kitty
            "마이멜로디": "https://i.imgur.com/L7p2Z0R.png", // My Melody
            "쿠로미": "https://i.imgur.com/X4yK7yH.png", // Kuromi
            "시나모롤": "https://i.imgur.com/2sR9y7d.png", // Cinnamoroll
            "폼폼푸린": "https://i.imgur.com/7bE9LpI.png", // Pompompurin
            "리틀 트윈스타 (키키&라라)": "https://i.imgur.com/Y1gRk6J.png", // Kiki-Lala
        }
        
        // 전역 변수 설정
        let nickname = "";
        let currentQuestionIndex = 0;
        let score = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };
        const totalQuestions = balanceGameData.length;

        /**
         * 게임 시작 버튼 클릭 시 호출되며, 별명을 저장하고 게임 화면을 표시합니다.
         */
        function startGame() {
            // 입력된 별명을 가져와서 공백 제거
            const input = document.getElementById('nickname-input').value.trim();
            if (input === "") {
                alert("별명을 입력해주세요! (예: 짱구)");
                return;
            }
            
            // 별명 저장
            nickname = input;
            
            // 화면 전환
            document.getElementById('nickname-screen').classList.add('hidden');
            document.getElementById('game-screen').classList.remove('hidden');
            
            // 총 질문 수 업데이트
            document.getElementById('total-questions').textContent = totalQuestions;
            
            // 첫 번째 질문 로드
            loadQuestion();
        }

        /**
         * 현재 질문 데이터를 화면에 로드합니다.
         */
        function loadQuestion() {
            // 현재 진행 상황 업데이트
            document.getElementById('current-index').textContent = currentQuestionIndex + 1;
            
            // 질문 데이터 가져오기
            const currentData = balanceGameData[currentQuestionIndex];
            
            // 텍스트 및 이미지 업데이트
            document.getElementById('current-question').innerHTML = 
                `<span class="text-xmas-gold text-4xl mr-2">🌟</span> ${currentData.question} <span class="text-xmas-gold text-4xl ml-2">🌟</span>`;
            
            // 옵션 A (index 0)
            document.getElementById('menu-a').textContent = currentData.options[0].name;
            document.getElementById('image-a').textContent = currentData.options[0].img;
            
            // 옵션 B (index 1)
            document.getElementById('menu-b').textContent = currentData.options[1].name;
            document.getElementById('image-b').textContent = currentData.options[1].img;
        }

        /**
         * 사용자가 옵션을 선택했을 때 호출됩니다.
         * @param {number} optionIndex - 선택된 옵션의 인덱스 (0: A, 1: B)
         */
        function selectOption(optionIndex) {
            const currentData = balanceGameData[currentQuestionIndex];
            const selectedOption = currentData.options[optionIndex];
            
            // 선택된 옵션의 MBTI 유형(type)에 점수 1점 부여
            // 예를 들어, 엽기 떡볶이(E)를 선택하면 score.E += 1
            score[selectedOption.type] += 1;
            
            // 다음 질문으로 이동
            currentQuestionIndex++;
            
            // 게임 종료 조건 확인
            if (currentQuestionIndex < totalQuestions) {
                // 다음 질문 로드
                loadQuestion();
            } else {
                // 결과 계산 및 화면 표시
                calculateResult();
            }
        }

        /**
         * 모든 질문이 끝난 후, 최종 MBTI와 결과를 계산하고 표시합니다.
         */
        function calculateResult() {
            // E/I, S/N, T/F, J/P 쌍별로 더 많이 선택된 유형을 결정
            // E vs I (E가 많으면 E)
            const type1 = score.E >= score.I ? 'E' : 'I'; 
            
            // S vs N
            const type2 = score.S >= score.N ? 'S' : 'N';
            
            // T vs F
            const type3 = score.T >= score.F ? 'T' : 'F';
            
            // J vs P
            const type4 = score.J >= score.P ? 'J' : 'P';

            // 최종 MBTI 문자열 생성
            const finalMBTI = type1 + type2 + type3 + type4;

            // 결과 매핑 데이터에서 최종 결과 가져오기
            const resultData = resultMappings[finalMBTI] || resultMappings.ELSE;

            // 화면 전환
            document.getElementById('game-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');

            // 닉네임, MBTI, 야식 결과 업데이트
            document.getElementById('result-nickname').textContent = `${nickname}님의 야식 MBTI는...`;
            document.getElementById('final-mbti').textContent = finalMBTI;
            document.getElementById('result-food-name').textContent = `😋 추천 야식: ${resultData.food} (${finalMBTI})`;
            document.getElementById('result-food-desc').textContent = resultData.desc;

            // 산리오 캐릭터 결과 업데이트
            const sanrio = resultData.sanrio;
            document.getElementById('sanrio-name').textContent = sanrio.name;
            document.getElementById('sanrio-desc').textContent = sanrio.desc;
            document.getElementById('sanrio-image').src = sanrioImages[sanrio.name] || sanrio.img; // 이미지 CDN 링크 사용
        }

        // 페이지 로드 시 초기 상태 확인 (별명 입력 화면만 보이게)
        window.onload = () => {
             // 페이지 로드 시 별명 입력창에 포커스
            document.getElementById('nickname-input').focus();
        };

    </script>
</body>
</html>
