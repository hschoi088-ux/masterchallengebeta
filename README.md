<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>구동사 스피드퀴즈 (Day 76-100)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            -webkit-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none;
            -webkit-touch-callout: none; -webkit-tap-highlight-color: transparent;
            word-break: keep-all; background-color: #f8fafc; font-family: system-ui, -apple-system, sans-serif;
        }
        .card { perspective: 1000px; }
        .card-inner {
            position: relative; width: 100%; height: 320px;
            transition: transform 0.6s; transform-style: preserve-3d; cursor: pointer;
        }
        .card.flipped .card-inner { transform: rotateY(180deg); }
        .card-front, .card-back {
            position: absolute; width: 100%; height: 100%;
            backface-visibility: hidden; display: flex; flex-direction: column;
            align-items: center; justify-content: center; border-radius: 1.5rem;
            padding: 2rem; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .card-back { transform: rotateY(180deg); }
        .active-tab { background-color: #4f46e5 !important; color: white !important; }
        .star-checked { color: #fbbf24 !important; fill: #fbbf24 !important; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body oncontextmenu="return false">

    <div class="max-w-md mx-auto px-4 py-6">
        <header class="text-center mb-6">
            <h1 class="text-2xl font-black text-indigo-600 tracking-tight italic">구동사 스피드퀴즈</h1>
            <p class="text-[10px] text-slate-400 mt-1 font-bold">DAY 076 - 100</p>
        </header>

        <div class="flex space-x-2 mb-6 overflow-x-auto pb-2 no-scrollbar font-bold text-xs">
            <button onclick="selectWeek('w16')" id="tab-w16" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100 active-tab">Day 76-80</button>
            <button onclick="selectWeek('w17')" id="tab-w17" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 81-85</button>
            <button onclick="selectWeek('w18')" id="tab-w18" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 86-90</button>
            <button onclick="selectWeek('w19')" id="tab-w19" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 91-95</button>
            <button onclick="selectWeek('w20')" id="tab-w20" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 96-100</button>
            <button onclick="selectWeek('total')" id="tab-total" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">전체 범위</button>
        </div>

        <div id="setup-screen" class="bg-white rounded-3xl p-8 shadow-xl border border-slate-100 text-center">
            <div id="selected-week-title" class="text-indigo-500 font-black text-xl mb-6 italic tracking-widest uppercase">DAY 76-80</div>
            <div class="space-y-4">
                <button onclick="startQuiz('mild')" class="w-full py-5 bg-emerald-500 text-white rounded-2xl font-black shadow-lg shadow-emerald-100 active:scale-95 transition">순한맛 시작</button>
                <button onclick="startQuiz('spicy')" class="w-full py-5 bg-orange-500 text-white rounded-2xl font-black shadow-lg shadow-orange-100 active:scale-95 transition">매운맛 시작</button>
            </div>
            <div class="mt-8 p-4 bg-slate-50 rounded-xl text-[11px] text-slate-400 text-left leading-relaxed">
                <p class="mb-2 text-indigo-600 font-bold underline">학습 모드 설명</p>
                <p>• <strong>순한맛</strong>: Model examples (교재1)</p>
                <p>• <strong>매운맛</strong>: 위 구성 + Small talk, Cases in point (교재 1, 2, 3)</p>
                <p class="mt-2 text-rose-400 font-bold italic">※ 선택 구간에서 랜덤 10문제가 출제됩니다.</p>
            </div>
        </div>

        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6 px-2">
                <span id="progress-text" class="px-3 py-1 bg-indigo-100 text-indigo-600 rounded-full text-xs font-bold">1 / 10</span>
                <button onclick="location.reload()" class="text-slate-400 font-bold text-xs uppercase">Exit ✕</button>
            </div>

            <div id="card-container" class="card" onclick="flipCard()">
                <div class="card-inner">
                    <div class="card-front bg-white border border-slate-100">
                        <span class="text-indigo-400 font-black text-[10px] tracking-widest mb-6 uppercase">Korean</span>
                        <p id="q-korean" class="text-lg font-bold leading-snug px-4 text-center"></p>
                        <p class="mt-10 text-slate-300 text-xs font-bold animate-pulse">터치하여 정답 확인</p>
                    </div>
                    <div class="card-back bg-indigo-600 text-white">
                        <button id="star-btn" onclick="toggleStar(event)" class="absolute top-6 right-6 text-white/40 transition-all">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-9 w-9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14l-5-4.87 6.91-1.01L12 2z"/>
                            </svg>
                        </button>
                        <span class="text-indigo-200 font-black text-[10px] tracking-widest mb-6 uppercase">English</span>
                        <p id="q-english" class="text-base font-black leading-snug px-4 mb-8 text-center overflow-y-auto max-h-[180px]"></p>
                        <button onclick="event.stopPropagation(); playTTS();" class="w-16 h-16 bg-white/20 rounded-full flex items-center justify-center active:scale-90 transition shadow-inner flex-shrink-0">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                            </svg>
                        </button>
                        <div id="q-info" class="absolute bottom-6 text-[9px] text-indigo-300 font-bold uppercase tracking-widest italic"></div>
                    </div>
                </div>
            </div>
            <button id="next-btn" onclick="nextQuestion()" class="w-full mt-8 py-5 bg-indigo-600 text-white rounded-2xl font-black shadow-xl hidden active:scale-95 transition">다음 문제 NEXT</button>
        </div>

        <div id="result-screen" class="hidden bg-white rounded-3xl p-6 shadow-xl border border-slate-100">
            <h2 class="text-center font-black text-xl text-slate-800 mb-8 tracking-tighter">학습 완료 리포트</h2>
            <div id="starred-list" class="space-y-4 max-h-[400px] overflow-y-auto pr-2 custom-scroll"></div>
            <button onclick="location.reload()" class="w-full mt-10 py-5 bg-slate-900 text-white rounded-2xl font-bold active:scale-95 transition">메인으로</button>
        </div>
    </div>

    <script>
        const allData = [
            // Day 76
            {d:76, s:"교재1", k:"나 방금 우리 (콘서트) 포스터 붙일 멋진 곳을 찾았어! 나 따라와 봐.", e:"I just found a great place to put up our posters! Come with me."},
            {d:76, s:"교재1", k:"나가기 전에 이 인테리어 사진들을 인스타에 올리자.", e:"Let’s put up these pictures of the decor on Instagram before we head out."},
            {d:76, s:"교재1", k:"보니까 시내에 신축 고층 건물을 올리는 모양이야. 다음 주에 공사 시작한대.", e:"Apparently, they will be putting up a new skyscraper downtown. I heard they’ll start construction next week."},
            {d:76, s:"교재1", k:"사촌이 (나를) 몇 주간 재워 주고는 있어. 이제는 다른 지낼 곳을 찾아야 해.", e:"My cousin has been putting me up for a few weeks. It’s time to find somewhere else to stay."},
            {d:76, s:"교재1", k:"(애완견을 잃어버린) 내 친구가 잃어버린 개를 찾기 위해 도시 곳곳에 전단지를 붙였다.", e:"My friend put up flyers all over the city to try and find her missing dog."},
            {d:76, s:"교재1", k:"그 카페가 최근에 빈티지 포스터를 붙였는데, 이 때문에 아늑하면서도 복고풍 감성이 더해졌다.", e:"The café recently put up vintage posters, giving it a cozy, retro vibe."},
            {d:76, s:"교재1", k:"이웃집 개들이 들어오지 못하도록 정원에 울타리를 칠까 해요.", e:"We were thinking of putting up a fence around our garden to keep out the neighbors’ dogs."},
            {d:76, s:"교재1", k:"보니까 박물관 있던 자리에 호텔을 지을 계획인가 봐요.", e:"Apparently, they’re planning to put up a hotel where the museum used to be."},
            {d:76, s:"교재1", k:"제 블로그에 숨이 막힐 정도로 멋진 일몰 사진을 올렸더니 바로 관심이 쏟아졌어요.", e:"I put up a breathtaking sunset photo on my blog, and it immediately got a lot of attention."},
            {d:76, s:"교재1", k:"저희 아파트를 리모델링하는 동안 Jeff가 몇 주간 재워 줬답니다.", e:"When my apartment was being renovated, Jeff put me up for a few weeks."},
            {d:76, s:"교재2", k:"이 반지 사진 찍어서 인스타에 올려도 될까?", e:"Can I take a picture of this ring and put it up on Instagram?"},
            {d:76, s:"교재2", k:"제발 그러지 마! 이건 내가 직접 한 디자인이잖아. 네 계정에 올리면 어떡해. (내가 디자인한 건데) 그렇게 하면 마치 네가 한 것처럼 보일 것 아냐.", e:"Please don’t! This is my personal design. You can’t put it up on your account. It’ll look like you’re taking credit for it."},
            {d:76, s:"교재2", k:"뭐 때문에 소음이 이리 심한 거야? (이런 지) 벌써 한참 됐어.", e:"What’s all that noise? It’s been going on for a while."},
            {d:76, s:"교재2", k:"저 아래쪽에 아파트 신축 공사를 하고 있어. 앞으로 몇 달 더 공사가 계속될 거야.", e:"They’re putting up a new apartment complex a few streets down. They’ll be working on it for the next few months."},
            {d:76, s:"교재2", k:"집에서 쓰지 않는 물건을 가져와서 자동차의 트렁크에 올려놓고 판매하는 것이 포스터 붙이는 것 좀 도와줄래? (우리가) 몇 주 있다가 카부츠 세일을 할 거라서.", e:"Hey, will you help me put up some of these posters? We’re going to have a car boot sale in a few weeks."},
            {d:76, s:"교재2", k:"포스터라고? 그거 효과 없을 텐데. 요즘 누가 포스터에 적힌 걸 보냐. 소문을 내려면 SNS에 올려야지.", e:"Posters? That’s not going to work. No one pays attention to what’s written on posters anymore. If you want to spread the word, you need to post it on social media."},
            {d:76, s:"교재3", k:"음, 문제가 좀 생겼다.", e:"Well, I have a bit of a problem."},
            {d:76, s:"교재3", k:"남동생이 최근에 안 좋게 이혼을 했는데, 내가 몇 주째 동생을 우리 집에 재워 주고 있다.", e:"My brother recently got out of a bad divorce, so I’ve been putting him up for the last few weeks."},
            {d:76, s:"교재3", k:"동생이 힘든 시간을 보내고 있어서 힘이 되어 주려고 한다.", e:"He’s been having a hard time, and I want to give him the support he needs."},
            {d:76, s:"교재3", k:"우리 애들도 다들 삼촌을 도우려고 노력 중이다.", e:"All of my kids have been trying to help him out."},
            {d:76, s:"교재3", k:"그런데 문제는, 그가 너무 오래 머물기 시작했다는 거다.", e:"But, here’s the thing, he’s starting to outstay his welcome."},
            {d:76, s:"교재3", k:"동생이 소파에 앉아서 하루 종일 TV만 보면서 먹기만 한다.", e:"All he does is sit on the couch all day watching TV and eating our food."},
            {d:76, s:"교재3", k:"동생 때문에 식비가 두 배나 늘었다.", e:"We’ve gone through twice as many groceries because of him."},
            {d:76, s:"교재3", k:"식비를 좀 보태겠다는 말도없다. 심지어 집안일도 도와주지 않는다.", e:"He hasn’t offered to help out with the grocery bill, or even pitch in with chores around the house."},
            {d:76, s:"교재3", k:"계속 이러면 얼마를 더 참으며 데리고 있을 수 있을지 모르겠다.", e:"If this keeps up, I don’t know how much longer we’ll be able to stand putting him up."},

            // Day 77
            {d:77, s:"교재1", k:"미네소타에서 몇 년을 살고 나서, 더는 겨울을 견디기 힘들다고 판단하고 플로리다로 이사했습니다.", e:"After years of living in Minnesota, I decided I couldn’t put up with the winters anymore, and moved to Florida."},
            {d:77, s:"교재1", k:"제 일이 너무 좋지만, 매일 2시간씩 걸리는 출퇴근을 언제까지 견딜 수 있을지 모르겠어요.", e:"I love my job, but I don’t know how long I can put up with this two-hour commute every day."},
            {d:77, s:"교재1", k:"계란 프라이도 못 한다고? (그럼) 네 아내가 너를 어떻게 참지?", e:"You can’t fry an egg? How does your wife put up with you?"},
            {d:77, s:"교재1", k:"저는 비행기 타는 걸 너무 싫어하고, 아내는 차 타고 오래 여행하는 걸 못 견딥니다. 그래서 그냥 집에서 휴가를 즐겨야 할 것 같네요.", e:"I hate flying, and my wife can’t put up with long road trips. Staycation it is, I guess."},
            {d:77, s:"교재1", k:"이 회사는 일하기 정말 좋은 곳이에요. 하지만 마감 시간을 못 지키는 건 용납하지 않습니다. 꼭 제시간에 업무한 걸 제출하셔야 해요.", e:"This company is a great place to work, but they don’t put up with missed deadlines. Make sure to submit your work on time."},
            {d:77, s:"교재1", k:"제가 십 대 때 부모님이 저를 어떻게 참아 주셨는지 모르겠어요. 정말 문제아였거든요.", e:"I don’t know how my parents put up with me during my teenage years. I was a handful."},
            {d:77, s:"교재1", k:"가끔씩 저를 참기 힘들다는 점은 인정합니다. 우리 팀에게 많은 것을 요구하지만, 그건 우리 모두가 최선을 다하기를 원하기 때문이에요.", e:"I admit that I’m hard to put up with at times. I demand a lot from my team, but it’s because I want us all to do our best."},
            {d:77, s:"교재2", k:"자기야, 이거 세일하네! 돈 아끼려면 두 개 사야겠다.", e:"Oh, Babe, it’s on sale! We should get two to save money."},
            {d:77, s:"교재2", k:"우리 예산으로는 당신의 낭비 습관을 감당하기 힘드네. 그리고 난 당신의 어린아이같은 모습을 참기 힘들어.", e:"Our budget can’t put up with your lavish spending, and I can’t put up with your childish logic."},
            {d:77, s:"교재2", k:"저희도 이사 가기는 싫습니다. 그렇지만 윗집 소음을 더는 참기 힘들어요. 임대 계약을 취소해야 할 것 같습니다.", e:"We don’t want to move, but we can’t put up with the noise coming from the upstairs neighbor any longer. We need to cancel our lease."},
            {d:77, s:"교재2", k:"이해합니다만, 그 집에 다시 이야기해 볼게요. 필요하다면 내보내겠습니다.", e:"I understand, but let me just talk to that neighbor again. I’ll remove him if I need to."},
            {d:77, s:"교재2", k:"(네) 여자 친구가 문자를 확인하는 데 3일이 걸린다.... 어떻게 참니?", e:"It takes her three days to check your text messages… How do you put up with her?"},
            {d:77, s:"교재2", k:"여자 친구가 나를 많이 아끼기는 해. 그냥 이 친구 소통 방식이 그래.", e:"I know she really cares about me. That’s just the way she communicates."},
            {d:77, s:"교재3", k:"룸메이트인 Steve를 만나기 전까지는 나는 늘 인내심이 많은 사람이었다.", e:"I had always been a patient person until I met my roommate, Steve."},
            {d:77, s:"교재3", k:"설거지를 안 한 그릇을 며칠이나 싱크대에 두지를 않나, 밤늦게 음악을 크게 틀지를 않나, Steve 이 친구는 자신이 너무 형편없는 룸메이트라는 걸 전혀 몰랐다.", e:"From leaving dirty dishes in the sink for days to blasting music late at night, Steve had no idea of how terrible a roommate he was."},
            {d:77, s:"교재3", k:"처음 몇 번은 이런 행동을 참으려고 했다.", e:"I tried to put up with his behavior the first couple of times."},
            {d:77, s:"교재3", k:"하지만 시간이 가면서 인내심이 바닥이 나기 시작했다.", e:"However, as time went on, I started to lose my cool."},
            {d:77, s:"교재3", k:"몇 번은 대놓고 따졌다. 하지만 아무것도 변하지 않았다.", e:"I confronted him several times, but nothing seemed to change."},
            {d:77, s:"교재3", k:"평일 밤에 파티를 하겠다고 했을 때, 나는 결국 한계에 달했다.", e:"Finally, I reached my breaking point when he decided to host a party on a week night."},
            {d:77, s:"교재3", k:"더는 이런 예의 없는 행동을 참을 수 없었다.", e:"I could no longer put up with such disrespect."},
            {d:77, s:"교재3", k:"새로운 룸메이트를 찾아야 할 시점이라고 판단했다.", e:"I decided it was time to find a new roommate."},

            // Day 78
            {d:78, s:"교재1", k:"구인 공고에 문제가 좀 생겼어요. 진행하기 전에 의견을 구해야 할 것 같아서요.", e:"I’ve run into some trouble advertising your job posting. I’ll have to get your input before proceeding."},
            {d:78, s:"교재1", k:"휴대폰을 보다가 결국 회전문에 부딪히고 말았어요.", e:"I was staring at my phone and ended up running into the revolving door."},
            {d:78, s:"교재1", k:"여기서 보다니 반갑다! 조만간 커피 한잔해야지!", e:"Nice running into you! We should grab a coffee soon!"},
            {d:78, s:"교재1", k:"착륙 직전에 난기류를 만났습니다.", e:"We ran into some turbulence just before landing."},
            {d:78, s:"교재1", k:"큰 문제만 없으면, 다음 달에 새 집에 들어갈 수 있을 거야.", e:"As long as we don’t run into any major issues, we should be able to move into our new house next month."},
            {d:78, s:"교재2", k:"Sally, 민수 보게 되면 문 앞에 택배 도착했다고 전해 줘.", e:"Sally, if you run into Minsu, tell him he has a package waiting at the entrance."},
            {d:78, s:"교재2", k:"알겠어! 민수 말이 나왔으니 말인데, 이번 주 내내 안 보이더라. 어디 갔지…? 아, 완전히 잊고 있었네! 내 정신 좀 봐. 지난주에 브리즈번으로 여행 갔지!", e:"Sounds good! Speaking of Minsu, I haven’t seen him all week. Where did he…? Oh, I totally forgot! I must be losing my mind. He went on a trip to Brisbane last week!"},
            {d:78, s:"교재2", k:"안녕하세요. 삼성페이에 문제가 생겼는데 좀 도와주실 수 있을까 해서요. 삼성페이로 강좌 등록을 시도하는데 이런 문제가 생겼습니다.", e:"Hello! There’s an error with my Samsung Pay, and I was hoping you could help me out with it. I ran into this problem while trying to sign up for the course using that app."},
            {d:78, s:"교재2", k:"불편을 드려 죄송합니다. 이런 일은 처음이네요. 한 시간 내로 다시 전화 드려도 될까요?", e:"Sorry for the inconvenience. We haven’t seen anything like this before. Do you mind if we get back to you in an hour or so?"},
            {d:78, s:"교재2", k:"John, 내가 이야기한 파일 구글 독스에서 볼 수 있어요. 내일까지 꼭 마무리 부탁해요. 문제 있으면 편하게 알려 주고요.", e:"John, you can access the file I mentioned in our Google Docs. Make sure to have it finished by tomorrow. Feel free to let me know if you run into any problems."},
            {d:78, s:"교재2", k:"바로 시작하겠습니다, 팀장님. 이사회 회의에 맞춰서 마무리하겠습니다.", e:"I’ll get right on it, sir. I will have it taken care of in time for the board meeting."},
            {d:78, s:"교재3", k:"겨우 작동만 되는 세탁기를 몇 년간 쓴 후에 드디어 최신 기능이 탑재된 LG 신형세탁기를 장만했다.", e:"After years of using a washing machine that barely works, I finally got a brand new LG washing machine with all the latest features."},
            {d:78, s:"교재3", k:"세탁기를 장만한 게 너무 신나서 배송을 기다리기가 힘들었다.", e:"I was so excited to get my hands on it that I could hardly wait for it to be delivered."},
            {d:78, s:"교재3", k:"그런데 오늘 아침에 배송이 되었을 때 설치 기사분들이 예상치 못한 문제에 부딪혔다.", e:"But then, when it finally came this morning, the guys installing it ran into a problem."},
            {d:78, s:"교재3", k:"너무 커서 우리 집 작은 세탁실에 안 들어갔다.", e:"It was too big to fit into our little laundry room."},
            {d:78, s:"교재3", k:"미리 공간을 쟀어야 했다.", e:"I guess we should have measured the space beforehand."},
            {d:78, s:"교재3", k:"좋다 말았다.", e:"I feel like I got excited for nothing."},
            {d:78, s:"교재3", k:"결국 매장에 다시 가서 우리 집 세탁실에 맞는 기본형을 골라야 할 것 같다.", e:"I think I’ll have to go back to the store and pick out a more basic model that will fit the space we have."},

            // Day 79
            {d:79, s:"교재1", k:"A: 앱솔루트 있나요?", e:"A: Do you have Absolute?"},
            {d:79, s:"교재1", k:"B: 아니요. 스미노프랑 스카이가 있습니다.", e:"B: No. We have Smirnoff and Sky."},
            {d:79, s:"교재1", k:"A: 그럼 그냥 스카이로 먹겠습니다.", e:"A: Then I will settle for Sky."},
            {d:79, s:"교재1", k:"A: 아보카도 샐러드 주세요.", e:"A: I’ll have the avocado salad."},
            {d:79, s:"교재1", k:"B: 죄송한데 아보카도가 다 떨어졌네요. 대신 치킨 샐러드 어떠실까요?", e:"B: Sorry, but we’re out of avocados. How about the chicken salad instead?"},
            {d:79, s:"교재1", k:"A: 뭐라고요? 정말요? 그럼 그냥 치킨 샐러드 주세요.", e:"A: What? Really? I guess I’ll just settle for the chicken salad, then."},
            {d:79, s:"교재1", k:"73분간 1점 차로 지고 있다가 환상적인 공격으로 동점을 만들었습니다. 시간만 조금 더 있었더라면 이길 수도 있었습니다. 하지만 무승부에 만족할 수밖에 없었네요.", e:"After being down by one for 73 minutes, we came back with a brilliant attack and tied it up. We probably could’ve won with a little more time, but we had to settle for a draw."},
            {d:79, s:"교재1", k:"원래는 좀 더 최신 모델을 원했는데 너무 비싸더라고요. 그래서 작년 모델에 만족해야 했어요.", e:"I wanted the newer model, but it was way out of my price range. I just settled for last year’s model, instead."},
            {d:79, s:"교재1", k:"정말 Rachel이랑 Dave를 위해 자리 마련해 주려고? Rachel은 아무 남자에게나 만족하지 않을 거라는 거 알면서.", e:"You really want to set Rachel up with Dave? You know she won’t settle for just any guy."},
            {d:79, s:"교재1", k:"신혼여행을 유럽으로 가고 싶었지만 여행 제한 시기에 결혼을 하는 바람에 제주도에 만족할 수밖에 없었다.", e:"We wanted to go to Europe for our honeymoon, but since we got married during travel restrictions, we had to settle for Jeju Island instead."},
            {d:79, s:"교재1", k:"마트에서 염소 치즈를 사려고 했지만 (없어서) 할 수 없이 페타 치즈에 만족해야만 했다.", e:"I was looking for goat cheese at the grocery store, but I had to settle for feta cheese instead."},
            {d:79, s:"교재1", k:"이번 주말에 원래는 한강에서 피크닉을 하고 싶었는데, 비가 오는 바람에 결국 할 수 없었다. 대신 보드게임 카페에서 체스 게임을 하는 것으로 만족해야만 했다.", e:"We were hoping to have a picnic by the Han River this weekend, but it ended up raining. We had to settle for playing chess at a board game café, instead."},
            {d:79, s:"교재2", k:"나 최근에 생긴 비건 피자 가게 가 볼까 하는데. 보니까 독특한 맛이 여러 가지 많은가 봐. 후기도 좋더라고.", e:"I’m planning on checking out that new vegan pizza place that just opened. Apparently, they have a lot of unique toppings. It’s been getting great reviews."},
            {d:79, s:"교재2", k:"근데 이 집 피자에는 고기가 안 올라가잖아. 두부나 고기 대용 재료에 만족해야 해.", e:"There’s no meat on their pizza, though. You would have to settle for tofu or meat substitutes."},
            {d:79, s:"교재2", k:"(제) 아내는 원래 의사나 변호사 같은 전문직인 사람과 결혼하길 원했어요. 그래서 처음에는 저 같은 회사원에 만족할 리가 없다고 생각했답니다.", e:"She always wanted to marry a professional, like a doctor or a lawyer. At first, I thought there was no way she would settle for an office worker like me."},
            {d:79, s:"교재2", k:"근데 어떻게 마음을 돌리게 만들었어요? 비결이 뭔가요? 두 분 지금은 천생연분 같아요!", e:"Then how did you get her to change her mind? What was your secret? You two seem like a match made in heaven now!"},
            {d:79, s:"교재2", k:"지난 주말에 정전이 장난이 아니었잖아요. 아내분이랑 결국 뭐 했어요?", e:"That power outage last weekend was crazy! What did you and your wife end up doing?"},
            {d:79, s:"교재2", k:"음, 원래는 집에서 넷플릭스 보면서 쉬려고 했는데, 정전이 되는 바람에 그냥 시간 보내려고 사진첩을 훑어봤어요.", e:"Well, our plan was to watch Netflix and chill at home, but since the power was out, we settled for looking through our photo albums to pass the time."},
            {d:79, s:"교재3", k:"내 드림카를 소유하는 것은 평생의 목표였다. 하지만 내가 생각하고 있던 모델은 너무 비싸다.", e:"Owning my dream car has been a lifelong goal, but the model I had in mind is way too expensive."},
            {d:79, s:"교재3", k:"그래도 내년에는 살 수 있는 방법을 찾아보려고 했다. 그런데 최근 딜러를 방문해 보고는 판단이 섰다.", e:"I was still trying to find some way to make the purchase next year, when a recent visit to the dealer gave me some perspective."},
            {d:79, s:"교재3", k:"딜러 측에서는 조금 더 저렴한 모델을 생각해볼 것을 제안했다. 프로 레이싱 같은 걸 원하는 게 아니라면 (조금 더 저렴한 모델로도) 나의 니즈를 충분히 충족시킬 것이라고 했다.", e:"They suggested I consider a less expensive model, saying it would more than meet my needs unless I wanted to do something like pro racing."},
            {d:79, s:"교재3", k:"고민을 좀 해 봤는데, 아무래도 이 대안에 만족해야 할 것 같다.", e:"After some thought, I think I will have to settle for this alternative."},
            {d:79, s:"교재3", k:"하긴 이 정도도 충분히 고급 모델이다. 이 모델을 가질 수 있는 것도 행운인 셈이다.", e:"It’s still a luxurious choice, after all, and I’d be lucky to own it."},

            // Day 80
            {d:80, s:"교재1", k:"A: Jerry는 늘 자신의 하버드 티셔츠를 입고 헬스장에 가더라.", e:"A: Jerry always wears his Harvard t-shirt to the gym."},
            {d:80, s:"교재1", k:"B: 맞아. 여자애들에게 잘 보이려고 그러는 거지. 하긴, 자랑할 근육은 없잖아.", e:"B: Yeah, he’s trying to show off to impress some girls. He has no muscles to show off, after all."},
            {d:80, s:"교재1", k:"A: 어젯밤 소개팅은 어땠어?", e:"A: How was that blind date you went on last night?"},
            {d:80, s:"교재1", k:"B: 최악이었어. 남자가 밤새 자기 자랑을 하더라고.", e:"B: Awful. The guy just bragged about himself the whole night."},
            {d:80, s:"교재1", k:"A: 나만 인스타에 올릴 멋진 게 없는 건가?", e:"A: Am I the only one who has nothing cool to post on Instagram?"},
            {d:80, s:"교재1", k:"B: 그 사람들(인스타에 멋진 사진을 올리는 사람들)은 그냥 과시하려고 그러는 거야. 너는 (굳이 안 그래도) 멋지잖아.", e:"B: Those people are just showing off. I think you’re cool."},
            {d:80, s:"교재1", k:"A: 자랑하려는 건 아니고, 올해 내 사업은 정말 상승세를 타고 있어.", e:"A: I don’t mean to brag about this, but my business has really taken off this year."},
            {d:80, s:"교재1", k:"B: 좋은 소식이다. 정말 잘됐다.", e:"B: That’s great news. I’m happy for you."},
            {d:80, s:"교재2", k:"어서 하와이에 가서 (나의) 새로 만든 비키니 몸매를 과시하고 싶어!", e:"I can’t wait to show off my new bikini body in Hawaii!"},
            {d:80, s:"교재2", k:"여보, 당신 틀림없이 멋져 보일 거야. 다이어트 정말 열심히 했으니까.", e:"I’m sure you’ll look amazing, honey. You’ve worked so hard on your diet."},
            {d:80, s:"교재2", k:"사람들이 롤렉스 시계에 열광하는 이유를 모르겠어.", e:"I don’t really get why everyone’s crazy about Rolex watches."},
            {d:80, s:"교재2", k:"음, 어떤 사람들에게는 (시계라는 것이) 단순히 시간을 확인하는 것 이상의 의미거든. 신분이자 부를 과시하는 것이지.", e:"Well, for some, it’s more than just telling time; it’s about status and showing off wealth."},
            {d:80, s:"교재2", k:"Nick이 (자기가) 아마추어 보디빌딩 대회에서 우승한 거 자랑하는 거 들었어?", e:"Did you hear Nick bragging about winning that amateur body building competition?"},
            {d:80, s:"교재2", k:"응, 최근에 운동 중독인 듯해. 그 친구 있는 데서 운동 이야기는 꺼내지 마. 몇 시간이고 계속 떠들 거야.", e:"Yeah, he seems addicted to workouts lately. Don’t even bring it up around him. He’ll go on for hours."},
            {d:80, s:"교재3", k:"구직자의 경우 면접 때 자신이 이룬 것을 과시하는 것이 중요합니다.", e:"As job seekers, it’s important to show off your achievements during interviews."},
            {d:80, s:"교재3", k:"그동안 거둔 성공과 능력을 자신 있게 강조해야 합니다.", e:"You should highlight your successes and skills with confidence."},
            {d:80, s:"교재3", k:"달리 말해, 자신이 이룬 것에 대해 자랑하는 것을 주저하지 마세요.", e:"In other words, don’t be shy about bragging about your accomplishments."},
            {d:80, s:"교재3", k:"(면접에서는) 어떻게 자신을 보여 주는지가 중요합니다. 너무 겸손하고 소심하면 자칫 자신감 없어 보일 수 있습니다.", e:"How you present yourself is important; if you are too humble or shy, you might come off as being less confident."},
            {d:80, s:"교재3", k:"요즘 고용 시장에서의 채용자들은 자신감 있는 사람에게 끌립니다.", e:"Employers in the job market today are attracted to confident people."},
            {d:80, s:"교재3", k:"자신이 그 자리에 적임자라고 판단되면 반드시 능력이 있다는 인상을 주셔야 합니다. 실제로 당신은 능력이 있으니까요.", e:"So as long as you believe you’re qualified for the position, make sure you come across as capable, because you are."},

            // Day 81
            {d:81, s:"교재1", k:"저는 오전에는 주로 게으름을 좀 피우며 동료들과 커피를 마시고 업무와 상관없는 기사를 읽습니다.", e:"I normally slack off a bit in the morning and have coffee with my colleagues, or read articles unrelated to work."},
            {d:81, s:"교재1", k:"더 이상 게으름 피우지 않으려고 노력 중입니다. ‘Plan Do See’라는 다이어리를 사용한 게 정말 큰 도움이 되었습니다.", e:"I’ve been trying to stop slacking off. Using this planner, called ‘Plan Do See’, has really helped."},
            {d:81, s:"교재1", k:"마라톤 코스가 10킬로미터 남은 지점에서 페이스가 떨어지는 게 느껴졌습니다.", e:"With 10 km left in the marathon, I could really feel my pace slack off."},
            {d:81, s:"교재1", k:"최근 들어 내 유튜브 구독자 수(증가세)가 주춤하고 있다. (다루는) 콘텐츠를 다시 생각해 봐야 할 것 같다.", e:"I noticed that my YouTube subscribers have been slacking off lately. Maybe I need to rethink my content."},
            {d:81, s:"교재1", k:"학기가 끝날 때쯤이면 학생들이 공부를 소홀히 하는 일이 잦다.", e:"Near the end of the school year, it’s common for students to slack off on their studies."},
            {d:81, s:"교재1", k:"저희 신규 신발 라인 매출이 주춤하고 있습니다.", e:"Sales of our new shoe line are slacking off."},
            {d:81, s:"교재1", k:"겨울이 다가오면서 일조량이 줄어들기 시작한다.", e:"As winter approaches, the amount of daylight begins to slack off."},
            {d:81, s:"교재2", k:"보니까 최근에 좀 느슨해지는 것 같군요. 무슨 문제가 있나요?", e:"I’ve noticed that you’ve been starting to slack off recently. Is something bothering you?"},
            {d:81, s:"교재2", k:"그냥 의욕이 식고 있어요. 새로 맡은 일에선 영어를 쓸 일이 많지 않아요. 그래서 매일 공부를 해야 할 이유를 모르겠어요.", e:"I’m just losing motivation. I don’t get to use English that much in my new role, so I don’t really see the point of studying every day."},
            {d:81, s:"교재2", k:"협력업체 납품이 점점 늦어지고 있어. 우리가 너무 편해서 좀 해이해지기 시작하는 듯해.", e:"Our supplier has been sending shipments later and later. It seems like they’re starting to slack off because they’re feeling comfortable with us."},
            {d:81, s:"교재2", k:"그냥 협력사를 바꿀까? 전화를 해서 계속 이런 식이면 다른 협력사로 바꾸는 걸 진지하게 고민할 거라고 경고를 해야 할 수도….", e:"Should we go ahead and try a new supplier? We can call and give them a warning that if this keeps up we will seriously consider switching…"},
            {d:81, s:"교재2", k:"지난해 영국에서 돌아온 후로 영어 실력이 심각하게 줄었어. 며칠 전에는 ‘cucumber(오이)’의 철자도 생각이 안 나더라니까.", e:"Ever since I got back from England last year, my English skill has slacked off significantly. The other day I couldn’t even remember the spelling of “cucumber.”"},
            {d:81, s:"교재2", k:"너무 속상하겠다. 근데 영어를 일상적으로 습관화하면 돼.", e:"That must be frustrating, but you just need to make English part of your daily routine."},
            {d:81, s:"교재3", k:"팀원 여러분,\n최근에 서비스와 음식 품질을 직접 확인하고자 사전 예고 없이 매장을 찾았습니다.", e:"Dear team,\nI recently visited our restaurant unannounced to check the service and food quality firsthand."},
            {d:81, s:"교재3", k:"일부 직원들이 해이해지고 있다는 걸 느꼈습니다.", e:"I noticed that some of our staff is starting to slack off."},
            {d:81, s:"교재3", k:"개점한 지 이제 겨우 3개월째입니다. 계속 긴장을 하는 것이 중요합니다.", e:"Since we have only been open for three months, it’s important to stay on our toes."},
            {d:81, s:"교재3", k:"이 메시지를 질책이 아닌 응원으로 받아들여 주시길 바랍니다.", e:"Please take this message as encouragement rather than blame."},
            {d:81, s:"교재3", k:"저는 최선을 다해 매장과 직원을 지원할 것을 약속합니다.", e:"Rest assured, I am committed to supporting our store and everyone involved to the best of my ability."},
            {d:81, s:"교재3", k:"다음 달에, 건너편에 새로운 버거 집이 생길 예정이니, 더더욱 긴장을 늦출 수 없습니다.", e:"With a new burger joint opening right across the street next month, we have even more reason not to let our guard down."},
            {d:81, s:"교재3", k:"계속 집중하고 개점할 때 우리 스스로 정한 높은 기준을 유지해 나갑시다.", e:"Let’s keep focused and maintain the high standards we set for ourselves when opening."},

            // Day 82
            {d:82, s:"교재1", k:"밖에 나가기 전에 집 안에서 재활용품을 분류해야 해. 일단 밖에 나가면 너무 추워서 분류하기 힘들어.", e:"Sort out all the recycling before going outside. It’ll be too cold to do it once you’re already out there."},
            {d:82, s:"교재1", k:"저도 점심을 함께 하고 싶지만 우선 책상에 널브러져 있는 서류부터 정리해야 할 것 같습니다.", e:"I’d like to join you for lunch, but first, I feel like I need to sort out all these papers on my desk."},
            {d:82, s:"교재1", k:"A: 문제가 좀 있다고 들었어. 다 잘 해결된 거야?", e:"A: I heard you were having some issues. Did you get everything sorted out?"},
            {d:82, s:"교재1", k:"B: 응, 물어봐 줘서 고마워. 상사가 도와줬어.", e:"B: Yeah, thanks for checking in. My boss helped me."},
            {d:82, s:"교재1", k:"최대한 빨리 휴가 일정을 짜야 해. 기다릴수록 가격이 더 올라갈 거야.", e:"We need to sort out our holiday itinerary as soon as possible. Things will get more expensive the longer we wait."},
            {d:82, s:"교재1", k:"서울 시민들은 쓰레기를 재활용, 음식물 쓰레기 그리고 일반 쓰레기로 분류해야 한다.", e:"Seoul residents are required to sort out their garbage into recyclables, food waste, and general waste."},
            {d:82, s:"교재1", k:"내 방이 정말 너무 엉망이야. 물건들을 좀 정리해야겠어.", e:"My room is a terrible mess, so I need to sort it out."},
            {d:82, s:"교재1", k:"겨울 옷 정리해야 해. (겨울에 입을 옷들을 꺼내 일부는 세탁소에 맡기고 일부는 버리는 등으로 정리하는 것을 의미)", e:"I need to sort out my winter clothes."},
            {d:82, s:"교재1", k:"정비사가 내 차 문제를 고쳤어. 알고 보니 짝퉁 벨트가 끼워져 있었더라고.", e:"The mechanic sorted out the problem with my car. Turns out I had a counterfeit belt."},
            {d:82, s:"교재1", k:"자, 헤어지기 전에 수지를 위한 깜짝 파티 계획을 짜야지. Sarah야, 네가 케이크 사 올 수 있지?", e:"OK, we need to sort out Suji’s surprise party before we leave. Sarah, can you pick up the cake?"},
            {d:82, s:"교재2", k:"미치겠네. 30분 후면 파티가 시작하는데, 거실이 무슨 태풍이 지나간 듯 어지럽혀져 있네!", e:"I’m starting to freak out. The party’s in 30 minutes, but the living room looks like a tornado hit it!"},
            {d:82, s:"교재2", k:"손님들 도착 전에 다 정리할게. 걱정 마.", e:"I’ll sort it out before our guests arrive. Don’t worry."},
            {d:82, s:"교재2", k:"한국인들이 미국인들보다 재활용 의식이 훨씬 더 높네요. 한국 오기 전에는 재활용품을 따로 분리해 본 적이 없거든요.", e:"Koreans are way more conscious of recycling than Americans. I never sorted out my recycling before I came here."},
            {d:82, s:"교재2", k:"맞아요. 미국인들이 집 앞에 아무렇게나 다 버리는 걸 보고는 정말 놀랐답니다.", e:"Absolutely. I couldn’t believe it when I saw Americans just dumping everything in front of their houses."},
            {d:82, s:"교재2", k:"어디 갔었어? 회의가 10분 전에 시작했는데.", e:"Where have you been? Our meeting started 10 minutes ago."},
            {d:82, s:"교재2", k:"2층 프린터가 고장 나서 여기 올라오기 전에 고치는 것 좀 도와주고 왔어.", e:"The printer wasn’t working on the second floor, so I had to help them sort it out before coming up here."},
            {d:82, s:"교재3", k:"제목: 공급 일정\n코튼 백의 현재 공급 현황에 대해 업데이트해 드리고자 합니다.", e:"Subject: Supply Timeline\nI’m writing to give you an update on our current supply of cotton bags."},
            {d:82, s:"교재3", k:"수요가 너무 많아서 공급 부족 사태가 발생했으며, 이 때문에 (공급에) 차질이 빚어지고 불편을 초래하게 되었습니다.", e:"Overwhelming demand led to shortages in supply, which has caused delays and inconveniences."},
            {d:82, s:"교재3", k:"문제를 해결하는 동안 너그러이 이해해 주시기 바랍니다. 하지만 원래 납기 예정일을 맞추는 데 문제가 없도록 할 것을 약속드립니다.", e:"We are asking for understanding while we sort out the issue, but we guarantee that it will be fixed in time to meet our previous delivery estimate."},
            {d:82, s:"교재3", k:"귀사의 인내심에 감사드립니다.", e:"Thank you for your patience."},

            // Day 83
            {d:83, s:"교재1", k:"좀 더 앉아 있다가 가자. 지금 나가면 차가 막힐 거야.", e:"Let’s stick around for a while. If we leave right now, we’ll get stuck in all the traffic."},
            {d:83, s:"교재1", k:"제가 볼 때는 간헐적 단식 (열풍)이 그리 오래 가지는 않을 것 같아요.", e:"I don’t think intermittent fasting will stick around for long."},
            {d:83, s:"교재1", k:"코로나가 끝나도 재택근무가 계속되기를 바랐는데, 그렇게 되지 않는 것 같아요.", e:"I had hoped that remote work would stick around after the pandemic, but it looks like that’s not happening."},
            {d:83, s:"교재1", k:"(클럽이) 너무 붐비기 전에 (서둘러) 클럽으로 가야 해. 여기는 30분 정도만 더 있자.", e:"We should head to the club before it gets too crowded, so let’s only stick around here for another 30 minutes or so."},
            {d:83, s:"교재1", k:"행사가 공식적으로는 9시에 끝나지만, 내빈들께서는 (행사가 끝난 뒤에도) 편하게 머무르시면서 서로 어울리셔도 됩니다.", e:"Although the event officially ends at 9, guests are welcome to stick around afterwards and mingle."},
            {d:83, s:"교재1", k:"아내가 고등어를 구울 때면 너무 싫어요. 처음부터 창문을 열어 두어도 냄새가 몇 시간이나 남아 있거든요.", e:"I hate it when my wife grills mackerel. Even though she opens the window first, the smell still sticks around for hours."},
            {d:83, s:"교재1", k:"가상현실 홈트레이닝이 요즘 인기지만 (이 열풍이) 그리 오래갈 것 같지는 않다.", e:"Virtual reality home workouts are popular now, but I don’t think they’ll stick around for long."},
            {d:83, s:"교재2", k:"나는 가 봐야겠다. 문 닫기 전에 명동 신세계에 들러야 하거든.", e:"I think I should get going. I need to swing by the Shinsegae in Myeongdong before they close."},
            {d:83, s:"교재2", k:"진짜로 지금 가려고? 6시밖에 안 됐는데. 좋아, 음, 난 내일 아침에 있을 중요한 발표 자료를 준비할 거라 조금 더 있다가 갈게.", e:"Are you sure you have to leave now? It’s just 6 o’clock. Alright, well, I’m going to stick around for a bit to work on this important presentation I have for tomorrow."},
            {d:83, s:"교재2", k:"유년 시절은 어땠나요?", e:"So, what was your childhood like?"},
            {d:83, s:"교재2", k:"음, 제가 태어난 지 얼마 안 돼서 아버지가 (가족을) 떠나 버렸어요. 그래서 한 부모 가정에서 자랐습니다. 외로울 때도 있었지만 굴하지 않으려고 노력했어요.", e:"Well, my dad didn’t stick around for long after I was born. That meant I was raised by a single parent. I felt isolated at times, but I tried not to let it get in my way."},
            {d:83, s:"교재2", k:"여보, 나 마라탕 식당 하나 차릴까 싶어. 사실 몇 군데 염두에 둔 곳이 있거든.", e:"Honey, I was thinking of opening a malatang restaurant. I actually already have a couple of locations in mind."},
            {d:83, s:"교재2", k:"정말 모든 걸 다 포기하고 사업 시작하겠다는 거야? 소상공인의 90% 가까이가 1년 안에 망한다고. 그리고 마라탕 열풍이 오래갈 것 같지도 않아.", e:"Are you sure you want to give everything up and start your own business? Almost 90% of small businesses go under within a year. And you know, I don’t think the malatang craze will stick around for long."},
            {d:83, s:"교재3", k:"신입 사원 시절에는 왜 선배 동료들이 업무를 다 마쳤는데도 퇴근을 안 하고 있는지 이해할 수 없었다. 그리고 회식은 왜 그리 많았는지.", e:"When I first started out, I couldn’t understand why my older co-workers would stick around the office after finishing their work, or why there were so many company dinners."},
            {d:83, s:"교재3", k:"시간이 흐르자 서로 친해지고 격려하기 위해 회식을 하는 게 매우 중요하다는 점을 깨달았다. 특히 힘든 하루를 보내고는 더더욱 그렇다.", e:"Over time, I’ve realized the importance of these dinners for bonding and supporting each other, especially after a tough day."},
            {d:83, s:"교재3", k:"이제는 긴장을 풀고 서로 유대를 쌓는 데는 동료들과 나가서 저녁을 먹는 게 최고라는 생각이 든다.", e:"Now, I believe there’s nothing like going out for dinner with colleagues to unwind and connect."},

            // Day 84
            {d:84, s:"교재1", k:"(그 회사) 인턴십이 어려운 건 알아. 하지만 인턴십이 끝날 때까지 포기하지 않으면 정규직이 될 거야.", e:"I know the internship is hard, but all you need to do is stick it out until the end and you’ll be a full-time employee."},
            {d:84, s:"교재1", k:"A: 아내가 갈비뼈 아래쪽이 아프다고 해서 병원에 데려갔어. 임신인데 임신 중기 이후로 (그 부위에) 엄청난 통증을 느끼고 있어.", e:"A: I took my wife to the doctor for pain under her ribs. She’s pregnant and has been getting sharp pains there ever since the second trimester."},
            {d:84, s:"교재1", k:"B: 아, 어떡해. 의사 선생님은 뭐래?", e:"B: Ah, that’s too bad. So, what did the doctor say?"},
            {d:84, s:"교재1", k:"A: 그냥 어떻게든 이겨 내는 수밖에 없대. 의사 선생님이 해 줄 수 있는 게 별로 없대.", e:"A: She basically just said that my wife needs to tough it out. There’s not much the doctor can do to fix it."},
            {d:84, s:"교재1", k:"이 프로젝트가 끝날 때까지 잘 버틸 수 있을지 모르겠어. 벌써부터 몸에 무리가 가는 게 느껴져.", e:"I don’t know if I’ll be able to stick it out through this project. It’s really taking a toll on me."},
            {d:84, s:"교재1", k:"(저희 부부는) 5년 전에 이혼을 하려고 했지만, 애들 대학 갈 때까지만 참기로 했답니다.", e:"We wanted to get divorced five years ago, but we decided to stick it out until our children made it to college."},
            {d:84, s:"교재1", k:"(남녀 간의) 이별을 견디기는 힘들지. 하지만 잘 이겨 내면 결국 더 강한 사람이 될 거야.", e:"Going through breakups is hard, but if you tough it out, you’ll be a stronger person in the end."},
            {d:84, s:"교재1", k:"시험이 힘들다는 건 선생님도 알아. 하지만 이제 거의 다 왔어! 힘들겠지만 몇 주만 더 버텨. 그러면 방학을 즐길 수 있을 거야.", e:"I know exams can be difficult, but you’re almost there! Just tough it out for a few more weeks and you’ll be enjoying your break."},
            {d:84, s:"교재2", k:"사표를 써야 할지 고민 중이야. 문제는 아직 다음 직장이 정해지지 않은 상태라는 거지.", e:"I’ve been debating whether I should just turn in my letter of resignation. The problem is, I don’t have another job lined up yet."},
            {d:84, s:"교재2", k:"당분간 먹고 살 수 있을 만큼 돈을 모아 두었잖아. 그냥 사표 쓰는 게 어때? (지금 회사에서) 더 참고 버티는 건 의미가 없는 것 같은데.", e:"I know you have some savings you could live off for a while, so why don’t you just turn it in? It doesn’t make sense to stick it out any longer there."},
            {d:84, s:"교재2", k:"박지성 선수, 에인트호번에서의 첫해가 어려웠다고 들었습니다. 그래도 잘 견뎌서 성공을 하셨지요. 이 부분에 대해 조금 더 말씀 부탁드려도 될까요?", e:"So, Mr. Jisung Park, I heard that your first year in Eindhoven was difficult, but you somehow managed to stick it out and become successful. Can you tell us a bit more about this?"},
            {d:84, s:"교재2", k:"물론입니다. 사실 정말 힘들었습니다. 그래도 견뎠습니다. 그리고 그 과정에서 동료들과 코치님들의 신망을 얻게 되었습니다.", e:"Sure. Well, the truth is, it was really tough. I stuck it out, and then ended up gaining the respect of my teammates and coaches along the way."},
            {d:84, s:"교재2", k:"싱가포르에서의 첫 여름을 잘 견뎠잖아. 어땠어?", e:"Well, you toughed out your first summer in Singapore ‒ how was it?"},
            {d:84, s:"교재2", k:"생각했던 것보다 훨씬 더 습했어. 정말 견디기 힘들었지. (시간이 지나면) 적응이 될지 잘 모르겠어.", e:"It was way more humid than I thought it would be. Totally unbearable. I don’t know if I’ll ever be able to get used to it."},
            {d:84, s:"교재3", k:"고등학교 1학년 때 미식축구를 했다.", e:"My first year in high school, I played American football."},
            {d:84, s:"교재3", k:"처음으로 정식 팀에서 뛴다는 생각에 설렜다.", e:"I was excited to play on a real team for the first time."},
            {d:84, s:"교재3", k:"하지만 얼마 가지 않아 알게 되었다. 방과 후 매일 같이 연습을 했는데, 95%는 달리기, 기구 들기, 기어다니기였고, 그러다 보면 구역질이 날 지경이 되었다.", e:"But I soon realized that football practice every day after school was 95% running, lifting weights and even crawling until I was ready to vomit."},
            {d:84, s:"교재3", k:"첫 주를 마친 후 아빠에게 그만두고 싶다고 했다. 하지만 아빠는 “참고 견뎌야 해. 시작한 건 마무리를 해야지.”라는 반응이었다. 그래서 그렇게 했다. 하지만 내가 더 나은 축구 선수가 된 것 같지는 않다.", e:"I told my dad I wanted to quit after the first week, but he was like, “You should stick it out and finish what you started.” I did, but I don’t think it made me a better football player."},
            {d:84, s:"교재3", k:"그래도 몸이 좋아진 건 분명했다.", e:"I sure did get in shape at least."},

            // Day 85
            {d:85, s:"교재1", k:"집에 오기 전에 은행에 들러 현금 좀 찾아 올 수 있을까?", e:"Before you come home, could you stop by the bank and get some cash?"},
            {d:85, s:"교재1", k:"추가 도움이 필요하면 수업이 끝난 후 제 사무실로 오세요.", e:"You can come by my office after class if you need extra help."},
            {d:85, s:"교재1", k:"오늘 오후에 시간이 좀 있어서, 프로젝트에 대해 논의하러 당신의 사무실에 들를 수 있습니다.", e:"I have some free time this afternoon, so I can drop by your office to discuss the project."},
            {d:85, s:"교재1", k:"오늘 밤에 내 생일을 축하할 거야. 바쁘지 않다면 네가 들러 주면 좋겠어.", e:"We’re celebrating my birthday tonight. It’d be nice if you could swing by, if you’re not busy."},
            {d:85, s:"교재1", k:"내일 내가 (매장이 있는) 그 동네 가거든. 너 대신 내가 그 매장에 들를게.", e:"I’ll be in the area tomorrow, so I’ll stop by the store for you."},
            {d:85, s:"교재1", k:"다들 보고 싶어 할 거야. 시간 될 때 들러.", e:"They would love to see you. You should come by for a visit sometime."},
            {d:85, s:"교재1", k:"Sanders 씨, 사무실 들어오는 길에 세탁소에 들러서 제 양복 좀 찾아 주실 수 있을까요?", e:"Mr. Sanders, on your way in to the office, could you please drop by the cleaners and pick up my suit?"},
            {d:85, s:"교재1", k:"집에 가는 길에 담배 사러 CU에 잠깐 들르려고 해.", e:"I think I’ll swing by CU on my way home for some cigarettes."},
            {d:85, s:"교재2", k:"소개팅에 시계를 빌려줘서 고마워. (시계를 착용하니) 패션이 완성되었어. 내일 너 집에 있으면 들러서 돌려줄게.", e:"Thank you for letting me borrow your watch for my blind date. It really made my outfit. I can stop by tomorrow to return it, if you’ll be home."},
            {d:85, s:"교재2", k:"아, 좋아. 1시까지는 집에 있을 거야. 오는 길에 미리 알려 줘.", e:"Oh, that would be nice. I’ll be home until 1 o’clock. Just give me a heads-up when you’re on your way."},
            {d:85, s:"교재2", k:"이번 주말에 조촐하게 저녁 파티를 할까 합니다. 오후 7시쯤 들르시겠어요? 그리스 음식을 준비할 계획이에요.", e:"I was thinking of hosting a small dinner party this weekend. Would you like to come by around 7 p.m.? I’m planning on cooking some Greek food."},
            {d:85, s:"교재2", k:"너무 좋네요! 와인 한 병 가져갈게요. 문자로 주소만 주시면 갈게요.", e:"That sounds lovely! I’ll bring a bottle of wine. Just text me your address, and I’ll be there."},
            {d:85, s:"교재2", k:"Brad, 별일 없지? 이따가 잠깐 들러도 될까 모르겠네. 노트북에 문제가 있어서 네가 좀 도와줬으면 해.", e:"How’s it going, Brad? I was wondering, could I swing by later? I’d like some help with my laptop."},
            {d:85, s:"교재2", k:"미안한데, 오후에 누가 오기로 되어 있어.", e:"Sorry, but I’m expecting some company* this afternoon."},
            {d:85, s:"교재3", k:"안녕하세요, 김 사장님. (집에) 들러서 (공사가) 잘 진행되고 있는지 한번 볼까 하는데요.", e:"Hello, Mr. Kim. I just thought I’d drop by and check in on how things are going."},
            {d:85, s:"교재3", k:"물론입니다. 사모님 댁 주방인걸요. 아직 싱크대 설치 중이라서 좀 지저분해 보일 수 있습니다.", e:"Sure. It’s your kitchen. We’re still installing the sink, so it might look a little messy."},
            {d:85, s:"교재3", k:"아, 괜찮습니다. 일 야무지게 잘하시는 것 압니다.", e:"Oh, I don’t mind. I know you guys do good work."},
            {d:85, s:"교재3", k:"말씀 감사합니다. 괜찮으시면 점심 시간 이후에 들러 주시면 제일 좋을 것 같습니다.", e:"I appreciate you saying so. It would be best if you dropped by after lunch, if that works for you."},
            {d:85, s:"교재3", k:"점심 이후 좋습니다. 그럼 곧 뵐게요.", e:"After lunch, it is. I’ll see you soon, then."},
            {d:85, s:"교재3", k:"좋습니다. 새로 꾸민 주방이 마음에 드실 겁니다.", e:"Sounds good. You’re going to be pleased with your new kitchen."},

            // Day 86
            {d:86, s:"교재1", k:"내가 거짓말을 했다고 부모님이 내 휴대폰을 빼앗아 갔다.", e:"My parents took my phone away for lying to them."},
            {d:86, s:"교재1", k:"그 친구 외모 빼면, 다른 사람이랑 별반 다를 게 없죠.", e:"If you take away her looks, she’s really just like everyone else."},
            {d:86, s:"교재1", k:"《도둑맞은 집중력》의 주제는 너무 좋았습니다. 제가 집중력에 심각한 문제가 있고, 잠도 잘 못 자거든요. 하지만 책에서 해결책은 얻지 못했어요.", e:"I loved the subject of Stolen Focus. I have some real problems focusing and getting enough sleep. However, I didn’t take away any solutions from it."},
            {d:86, s:"교재1", k:"날씨가 안 좋아서 해변에서의 휴가 즐거움이 반감되었다.", e:"The bad weather really took away from our beach vacation."},
            {d:86, s:"교재1", k:"바구니에 다섯 개의 사과가 있어. 이 중 두 개를 빼면 몇 개가 남을까?", e:"You have five apples in a basket. How many are left if I take away two of them?"},
            {d:86, s:"교재1", k:"이 음식은 주문 안 했어요. 가져가 주시겠어요?", e:"I didn’t order this dish. Could you please take it away?"},
            {d:86, s:"교재1", k:"John이 어젯밤에 또 음주 운전으로 걸렸어. 이번이 세 번째라서 경찰이 결국 면허증을 압수해 버렸지.", e:"Last night, John was pulled over after drinking and driving again. It was his third time, so they finally took away his license."},
            {d:86, s:"교재1", k:"영화를 본 후 우울한 기분이 들었다(= 우울한 기분으로 영화관을 나왔다).", e:"I took away a feeling of melancholy after watching the movie."},
            {d:86, s:"교재1", k:"3시간 강좌 후에 선생님이 강좌에서 가장 인상 깊었던 핵심 내용 목록을 만들라고 했다.", e:"After a 3-hour lecture, the teacher asked us to make a list of the key ideas we took away from the lesson."},
            {d:86, s:"교재1", k:"친구가 결말을 이야기하는 바람에 소설의 스릴이 반감되었다.", e:"My friend spoiled the ending, which really took away from the novel’s suspense."},
            {d:86, s:"교재1", k:"파티가 제법 좋았지만, (파티를) 주최한 부부가 거의 파티 내내 싸우는 바람에 즐거움이 반감되었다.", e:"The party was pretty good, but the couple that was hosting fought pretty much the whole time. That took away from our enjoyment."},
            {d:86, s:"교재1", k:"연극은 너무 좋았지만, 좌석이 불편해서 연극 관람을 망쳤다.", e:"Even though the play was fantastic, the uncomfortable seats took away from the overall experience."},
            {d:86, s:"교재2", k:"우리 아들이 영어 학원을 가야 할 때 PC방에 몰래 가는 걸 봤어. 어떻게든 혼을 내야 하는데, 어떻게 해야 할지 모르겠네.", e:"I caught my son sneaking off to the PC room when he was supposed to be going to his English academy. I need to punish him somehow, but I’m not really sure what to do."},
            {d:86, s:"교재2", k:"휴대폰을 뺏고, 몇 주간 컴퓨터를 못하게 하는 건 어때? 그래야 정신 차릴 것 같은데.", e:"What about taking away his phone and computer benefits for a few weeks? Maybe that will teach him a lesson."},
            {d:86, s:"교재2", k:"이효준이 출연한 신작 드라마 봤어? 이효준은 정말 너무 멋져.", e:"Did you see that new show that Lee Hyojun starred in? I just can’t get enough of him."},
            {d:86, s:"교재2", k:"정말? 난 좀 별로던데. 외모 빼면 다른 사람이랑 별 차이 없는데. 아무리 좋게 이야기해도 연기도 그저 그랬고.", e:"Seriously? I was pretty unimpressed. I mean, if you take away his looks, he’s just like everyone else. His acting was mediocre, to say the least."},
            {d:86, s:"교재2", k:"브리즈번에 있는 그 해산물 식당에서의 저녁은 어땠어?", e:"How was your dinner at that seafood place in Brisbane?"},
            {d:86, s:"교재2", k:"해산물은 신선했어. 그런데 가격이 터무니없어서 식사를 망쳤어. 한 입 먹을 때마다 ‘너무 바가지’라는 생각이 들더라고!", e:"The seafood was fresh, but the outrageous prices took away from the meal. I was thinking “what a rip-off” with every bite!"},
            {d:86, s:"교재3", k:"정부는 제가 운전면허증을 반납해야 한다고 합니다.", e:"The government says I have to return my driver’s license."},
            {d:86, s:"교재3", k:"이들의 주장에 따르면 85세인 제가 운전하는 것은 위험하다고 합니다. 하지만 저는 전혀 동의할 수 없습니다.", e:"They claim it’s dangerous for me to drive at age 85, but I completely disagree."},
            {d:86, s:"교재3", k:"제가 운전하기에 부적합하다는 점을 증명할 수 있는 테스트 같은 건 없습니다. 그냥 무조건 반납하라고 합니다.", e:"There’s no test to prove that I’m an unfit driver; they’re just demanding it back!"},
            {d:86, s:"교재3", k:"제 면허증을 빼앗아 가는 건 도저히 이해가 되지 않습니다.", e:"Taking away my driver’s license makes no sense."},
            {d:86, s:"교재3", k:"저는 평생을 건강하게 살았고, 항상 규칙적으로 운동했습니다.", e:"I’ve been healthy my entire life and have always exercised regularly."},
            {d:86, s:"교재3", k:"정신도 멀쩡합니다. 안경도 필요 없습니다. 틈날 때마다 조깅도 하고 헬스도 합니다.", e:"My mind is still sharp, I don’t need glasses, and I even jog and lift weights in my spare time."},
            {d:86, s:"교재3", k:"저자신을 운전하기에 매우 건강하다고 생각하지만, 정부는 아랑곳하지 않습니다.", e:"I consider myself fit enough to drive, but the government doesn’t care."},
            {d:86, s:"교재3", k:"이 청원에 동참해 주세요.", e:"Please sign this petition."},

            // Day 87
            {d:87, s:"교재1", k:"A: 숙모님한테 빌린 신발 돌려 드렸어?", e:"A: Did you take those shoes back to your aunt after you borrowed them?"},
            {d:87, s:"교재1", k:"B: 아, 아니! 알려 줘서 고마워. 잊어버리기 전에 지금 가져다드려야겠다.", e:"B: Oh, no! Thanks for the reminder. I’ll take them back now, before I forget again."},
            {d:87, s:"교재1", k:"잠시만, 내가 한 말 취소할게. 그런 의도는 아니었어. 내가 “피곤해 보인다”라고 했을 때 “못생겨 보인다”라는 말은 아니었어. 그냥 네가 아플까 봐 걱정된 것뿐이야.", e:"Wait, I take that back. I didn’t mean it like that. When I said, “You look tired,” I didn’t mean it like “You look ugly.” I’m just concerned about you getting sick."},
            {d:87, s:"교재1", k:"나는 그녀가 자기가 입던 스웨터를 나한테 준 거라고 생각했는데, 그냥 빌려준 거더라고. 몇 주간 빌려준 다음 도로 가져갔어.", e:"I thought she gave me her old sweater, but it turned out she only wanted to lend it. She let me use the sweater for a few weeks, and then she took it back."},
            {d:87, s:"교재1", k:"이 사진들을 보니 옛날 생각이 나네요. 지금 보면 (저의) 텍사스 시절이 근심 걱정없어 보이지만, 사실은 그렇지 않았어요. 당시에는 미래에 대한 스트레스가 꽤 컸습니다.", e:"Looking at these pictures really takes me back. Those days in Texas seem so carefree now, but I know it’s not real. At the time, I was pretty stressed out about my future."},
            {d:87, s:"교재1", k:"이 스웨터를 반품하시려면 영수증이 있어야 합니다.", e:"You need the receipt if you want to take your sweater back."},
            {d:87, s:"교재1", k:"네가 바보 같다고 해서 미안해. 취소할게.", e:"I’m sorry that I said you were stupid. I take it back."},
            {d:87, s:"교재1", k:"네가 나한테서 훔쳐 간 돈 내가 다시 가져갈 거야.", e:"I’m going to take back the money you stole from me."},
            {d:87, s:"교재1", k:"어젯밤에 사고 장면이 떠오르는 꿈을 꿨어요.", e:"I had a dream last night that really took me back to the scene of the accident."},
            {d:87, s:"교재2", k:"죄송한데, 웰던 스테이크를 시켰는데, 이건 미디엄 레어 같네요.", e:"Excuse me, but I asked for a well-done steak, and this looks like it’s medium rare."},
            {d:87, s:"교재2", k:"죄송합니다. 다시 가져가서 바로 다른 것으로 만들어 오겠습니다.", e:"Oh, I’m sorry about that. I’ll take it back and get another one cooked for you right away."},
            {d:87, s:"교재2", k:"확실하지는 않은데 열흘 전쯤에 여기서 캐시미어 스웨터를 구매한 것 같거든요. 혹시 반품하고 환불을 받을 수 있을까 해서요. 아니면 적립금으로 받거나….", e:"I’m not sure, but I think I bought this cashmere sweater from you guys like 10 days ago. I just wanted to know if I could take it back and still get a refund, or maybe store credit…"},
            {d:87, s:"교재2", k:"죄송하지만, 그럴 경우 환불이 어렵습니다. 저희 환불 정책에 따르면 구매 시점부터 일주일 안에만 환불받으실 수 있습니다.", e:"Sir, in that case I’m afraid you can’t receive a refund. Our refund policy states that you are only eligible for one within a week of purchase."},
            {d:87, s:"교재2", k:"이런 노래를 들으면 십 대 때로 돌아간 것 같아. ‘블랙핑크’와 ‘뉴진스’ 노래도 좋지만, 솔직히 가사에 공감이 잘 안되거든.", e:"There is something about songs like this that takes me back to my teens. I like Blackpink and New Jeans songs, but I can’t really relate to the lyrics, honestly."},
            {d:87, s:"교재2", k:"이런… 이런 노래를 틀 때마다 잊고 있던 우리 나이 차가 다시 생각나. 자기 뭐, 할아버지야? 나보다 어린 남자랑 만나야 하는 건데….", e:"Oh my gosh… you know, every time you play songs like these it reminds me of our age gap. What are you, a grandpa? I wish I could date someone younger than me..."},
            {d:87, s:"교재3", k:"TV로 야구를 볼 때마다 텍사스에서의 시절이 생각난다.", e:"Watching baseball on TV always takes me back to my time in Texas."},
            {d:87, s:"교재3", k:"(인생의) 다음 행보를 어떻게 가져가야 할지를 몰랐기 때문에 혼란스러운 시기였고, 미래에 대한 스트레스가 심했다.", e:"It was a confusing time because I couldn’t figure out my next move, and I was stressed out about my future."},
            {d:87, s:"교재3", k:"당시에는 야구 보는 게 큰 낙이었다.", e:"Baseball was a great distraction for me back then."},
            {d:87, s:"교재3", k:"경험이 없다 보니 군대에 가거나 코딩 같은 것을 기초부터 배우는 것 외에는 할 수 있는 선택지가 없었다.", e:"There was nothing I could do with my lack of experience, except maybe join the military or learn something, like coding, from the ground up."},
            {d:87, s:"교재3", k:"LG에 근무했던 삼촌이 인턴 자리를 소개해 주면서 내 삶이 안정을 찾았다.", e:"My uncle, who worked for LG, really got me on the right track by getting me that internship."},
            {d:87, s:"교재3", k:"지금 생각하면 (텍사스에서의 생활에) 그리운 면이 있기는 하다. 하지만 지금의 상황이 최선의 결과다.", e:"Now, there are some things I miss, but I know things have worked out for the best."},

            // Day 88
            {d:88, s:"교재1", k:"A: 곧 출발하나요?", e:"A: Are you taking off soon?"},
            {d:88, s:"교재1", k:"B: 네, 태워드릴까요?", e:"B: Yeah, do you need a ride?"},
            {d:88, s:"교재1", k:"포켓몬 고 열풍이 불었을 때 다 큰 어른들이 길을 걸으면서 포켓몬을 보던 거 기억해 봐요.", e:"Remember when Pokemon GO took off, and grown men were walking down the streets looking for Pokemon."},
            {d:88, s:"교재1", k:"바지 두 벌 사면 15% 빼 준대.", e:"If I buy two pairs of pants, they’ll take 15% off."},
            {d:88, s:"교재1", k:"너는 안경 벗으면 완전 딴 사람 같아 보여.", e:"You look like a different person when you take off your glasses."},
            {d:88, s:"교재1", k:"태풍으로 인해 6시간 동안 연착이 된 터라, 15분 후에 비행기가 이륙한다는 기장의 안내 방송은 너무나 다행스러웠다.", e:"After a six-hour typhoon delay, it was such a relief to hear the captain announce that the plane was going to take off in 15 minutes."},
            {d:88, s:"교재1", k:"(같이 있는 두 사람이 다른 곳에서 출발하는 친구에게 보내는 메시지) 우리는 지금 출발하려고 해. 그래야 영화관에 늦지 않게 도착할 수 있어.", e:"We’re going to take off so we can make it to the movie on time."},
            {d:88, s:"교재1", k:"탕후루가 몇 달 동안 엄청나게 인기였는데, 이제 인기가 시들해지고 있어.", e:"Tanghuru really took off for a few months, but it’s starting to lose popularity."},
            {d:88, s:"교재1", k:"이 물건들은 선반에서 빼야 합니다. 유통기한이 지났어요.", e:"We need to take these items off the shelf. They’re expired."},
            {d:88, s:"교재1", k:"현금으로 결제하시면 10달러를 더 빼 드리겠습니다.", e:"If you pay in cash, we will take an additional ten dollars off."},
            {d:88, s:"교재2", k:"정말 지금 가려고? 아직 할 이야기가 많은데.", e:"Are you sure you’re really going to leave now? We still have a lot to catch up on."},
            {d:88, s:"교재2", k:"미안해, 근데 지금 일어나 봐야 해. 남편이 식당에서 기다리고 있어서 말이야.", e:"Sorry, but I have to take off now. My husband is waiting for me at the restaurant."},
            {d:88, s:"교재2", k:"네가 볼 땐 K팝 열풍이 얼마나 오래갈 것 같아? 내가 보기에는 일시적인 유행일 것 같아.", e:"How long do you think the K-pop craze will stick around? I’m afraid it’s going to be just a fad."},
            {d:88, s:"교재2", k:"K팝과 K드라마가 본격적으로 인기를 끈 게 이제 겨우 몇 년이라서, 나는 아직은 크게 걱정 안 해.", e:"It’s been only a couple of years since K-pop and K-dramas really took off, so I’m not too worried about it yet."},
            {d:88, s:"교재2", k:"John, 힘이 없어 보이네. 무슨 일 있는 거야?", e:"John, you look down. What’s up with you?"},
            {d:88, s:"교재2", k:"정말 화가 나. 또 승진에서 누락됐거든. 팀장이 마지막 순간에 나를 최종 후보군에서 빼 버렸어. 뭔가 나한테 악감정이 있나 봐.", e:"I’m really pissed off. I’ve been passed over for a promotion again. It turned out my team leader took me off the shortlist of candidates at the last minute. He must have something against me."},
            {d:88, s:"교재3", k:"중고 거래 사이트에서 이것저것 보고 있었는데 이 멋진 가죽 코도반 신발을 발견했다.", e:"I was messing around on a used market site when I came across these nice leather cordovan shoes."},
            {d:88, s:"교재3", k:"완전 새것 같았다. 거의 110만 원 정도 하는 구두다.", e:"They seemed brand new, and they are worth almost 1.1 million won."},
            {d:88, s:"교재3", k:"판매자가 58만 원에 올려 두었는데, 정말 저렴하다고 생각했다.", e:"He listed the shoes for 580,000 won, which I thought was a real bargain."},
            {d:88, s:"교재3", k:"(판매자가 있는) 신사동에 가서 직접 픽업하겠다고 했더니 3만 원을 더 빼 주었다!", e:"Since I offered to go to Sinsa-dong and get them myself, he even took off an extra 30,000!"},
            {d:88, s:"교재3", k:"이런 신발을 55만 원에 가지게 되다니 정말 횡재했다.", e:"I really lucked out getting these shoes for just 550,000 won."},
            {d:88, s:"교재3", k:"이 (코도반) 가죽을 길들이려면 얼마나 걸릴지 하는 점이 딱 하나 걱정되는 부분이다.", e:"The only thing I’m worried about now is how long it’ll take to break in this leather."},

            // Day 89
            {d:89, s:"교재1", k:"여름 여행 계획 짜는 건 내가 맡을 수 있을 것 같아. 너희들이 가 보고 싶은 곳이 있으면 보내 줘.", e:"I think I could take on planning our trip for next summer, so send me your ideas of what to check out."},
            {d:89, s:"교재1", k:"휴가철을 맞아 추가 직원을 뽑는데, 혹시 겨울방학 동안 아르바이트할 사람 알고 있어?", e:"We’re taking on additional staff for the holiday season. Do you know anyone who’d want a part-time job over winter vacation?"},
            {d:89, s:"교재1", k:"그녀는 사무실에서는 완전 딴 사람이 된다. 농땡이 부리는 모습은 찾아보기 힘들다.", e:"She takes on a whole different personality when she’s in the office. Her goofy character completely disappears."},
            {d:89, s:"교재1", k:"대학생들이 많은 빚을 지게 되는 건 너무 흔한 일이다. 이 방법 말고는 폭등한 등록금을 마련할 길이 없다.", e:"It’s all too common for college students to take on lots of debt. There’s pretty much no other way to come up with the inflated tuition."},
            {d:89, s:"교재1", k:"아시안컵에서 한국이 이란과 붙었다. 아쉽게도 지고 말았다.", e:"Korea took on Iran in the Asian cup, and unfortunately lost."},
            {d:89, s:"교재1", k:"이제 주장이 되었으니 더 많은 리더 역할을 맡아야 할 것 같습니다.", e:"Now that I’m the team captain, I’ll have to take on more leadership duties."},
            {d:89, s:"교재1", k:"내가 일을 너무 많이 떠맡았어. 이 프로젝트 좀 도와줄 수 있을까?", e:"I have taken on too much work. Can you please help me with this project?"},
            {d:89, s:"교재1", k:"추가 학생은 못 받을 것 같네요. 이미 제가 편하게 감당할 수 있는 학생 수를 넘어섰어요.", e:"I don’t think I could possibly take on anyone else. I already have more students than I’m comfortable with."},
            {d:89, s:"교재1", k:"그녀는 남자 친구와 있을 때 딴사람이 된다.", e:"She takes on a different personality when she is with her boyfriend."},
            {d:89, s:"교재1", k:"부모님이 사고로 돌아가셔서 제가 부모님 빚을 다 떠안을 수밖에 없었습니다.", e:"I was forced to take on my parents’ debt when they passed away in an accident."},
            {d:89, s:"교재2", k:"Joe가 너보다 축구를 훨씬 더 잘한다고 계속 허세를 부리네. 너 가만히 있을 거니?", e:"Joe keeps on bragging about how he’s so much better than you at soccer. Are you going to do anything about it?"},
            {d:89, s:"교재2", k:"뭐라고? 그런 말 같지 않은 소리를 한다고? 좋아, 학교 마치고 내가 1 대 1로 상대해주겠다고 전해 줘.", e:"What? He’s still talking trash? Sure, tell him I’ll take him on 1-on-1 after school."},
            {d:89, s:"교재2", k:"여보, 지금 사람을 더 쓰는 건 좋은 생각 같지가 않아. 비용 중에 인건비가 제일 크기도 하고, 믿을 수 있는 사람 찾는 것도 쉽지 않을 거야.", e:"Honey, taking on more staff right now doesn’t seem like a good idea. Labor is already our biggest expense, and finding someone we trust is going to be tough."},
            {d:89, s:"교재2", k:"알아. 근데 손님이 너무 많잖아. 이제 우리가 나이도 있고, 우리끼리 이 모든 걸 하는 건 너무 버거워.", e:"I get that, but we’re swamped with customers. We’re not as young as we used to be, and doing all this on our own is becoming too much."},
            {d:89, s:"교재2", k:"Sarah가 남자 친구 생겼다고 들었어. 친해지기 힘든 성격이라 놀랐네. 남자 친구는 만나 본 거야?", e:"I heard Sarah has a boyfriend. I was surprised, since she’s kind of hard to get along with. Have you met him?"},
            {d:89, s:"교재2", k:"응, 어제 쇼핑몰에서 우연히 봤어. 믿기지는 않겠지만 남자 친구 옆에 있으니 완전히 딴사람이 되더라. 남자 친구가 그걸 언제쯤 눈치채려나 몰라.", e:"Yeah, I bumped into them at the mall yesterday. You wouldn’t believe it, but she takes on a completely different personality around him. I wonder how long it will take him to catch on."},
            {d:89, s:"교재3", k:"한꺼번에 여러 가지 일을 맡는 건 이해는 됩니다. 하지만 많은 분이 눈치채셨겠지만, 한 번에 감당할 수 있는 일은 한계가 있습니다.", e:"While taking on multiple projects makes sense, a lot of you out there have probably figured out that there is only so much you can handle at one time."},
            {d:89, s:"교재3", k:"한꺼번에 여러 프로젝트를 맡게 되면 일에 누수가 생기고, 스트레스는 커지며, 번아웃이 옵니다.", e:"Try to take on too many projects and things get missed, stress builds up, and people burn out."},
            {d:89, s:"교재3", k:"다섯 개 이상의 프로젝트를 하는 직원들은 집중력 저하와 뇌의 부하를 경험할지도 모릅니다.", e:"Employees with more than five projects might experience lack of focus and stretched brain capacity."},
            {d:89, s:"교재3", k:"따라서 가능하다면 한꺼번에 다섯 개 이상의 일을 맡지 마시고, 직원들에게도 그 이상의 일을 동시에 하게끔 압박해서는 안 됩니다.", e:"So, if possible, don’t take on more than five projects at a time and don’t push your employees to juggle more than that, either."},

            // Day 90
            {d:90, s:"교재1", k:"올해 처음으로 에어컨을 사용하기 전에 반드시 필터를 빼서 청소를 하렴.", e:"Before you use your air conditioner for the first time this year, don’t forget to take out the filter and wash it."},
            {d:90, s:"교재1", k:"나가는 길에 쓰레기 좀 버려 줄 수 있어?", e:"Can you take out the garbage on your way out?"},
            {d:90, s:"교재1", k:"그분들이 청담에 있는 고급 식당에 가서 식사 대접을 해 주더군요.", e:"They took me out to a fancy restaurant in Cheongdam."},
            {d:90, s:"교재1", k:"이 아파트를 사기 위해 대출을 좀 받아야만 했습니다.", e:"I had to take out some loans to buy this apartment."},
            {d:90, s:"교재1", k:"A: 엄마, 김밥에 들어 있는 시금치 너무 싫어요.", e:"A: Mom, I really hate spinach in my kimbap."},
            {d:90, s:"교재1", k:"B: 음, 그렇다고 절대로 시금치를 빼서 버리면 안 돼. 시금치는 몸에 정말 좋아.", e:"B: Well, I won’t let you take it out and waste it. Spinach is really good for you."},
            {d:90, s:"교재1", k:"이 문장은 빼세요. 전체 내용과 어울리지 않네요.", e:"I want you to take out this line. It looks out of place with the rest."},
            {d:90, s:"교재1", k:"차를 (전시장에서) 가지고 나가서 한 바퀴 돌아 봤는데요. 성능이 끝내주더군요.", e:"I took it out for a spin and was blown away by its performance."},
            {d:90, s:"교재1", k:"음식물 쓰레기를 내다 버리는 건 제일 하기 싫은 집안일이다.", e:"Taking out the food waste is my least favorite house chore."},
            {d:90, s:"교재1", k:"부모님이 서울에 계실 동안 모시고 나가서 저녁 식사를 대접할까하는데, 서울역 근처에 괜찮은 식당 알아?", e:"I was thinking of taking my parents out to dinner while they are in town. Do you know of any nice places near Seoul Station?"},
            {d:90, s:"교재1", k:"모은 돈이 부족해서, 새 차 살 때 대출을 받아야만 했어요.", e:"I didn’t have enough saved up, so I had to take out a loan to buy my new car."},
            {d:90, s:"교재1", k:"나한테 화풀이하지 마!", e:"Don’t take your anger out on me!"},
            {d:90, s:"교재2", k:"아들, 이제 공부 그만하고 자야지. 시간이 늦었어. 자기 전에 콘택트렌즈 빼는 거 잊지 말고.", e:"Honey, stop studying and go to bed. It’s already late. And don’t forget to take out your contacts before you sleep."},
            {d:90, s:"교재2", k:"엄마, 제가 기말시험이 일주일도 안 남았다고 말했잖아요. 밤새야 할 것 같아요.", e:"Mom, didn’t I tell you that my finals are less than a week away? I think I’ll have to pull an all-nighter."},
            {d:90, s:"교재2", k:"오늘 저녁에 고객분들이 사무실에 오는데 회의 끝나고 저녁 식사를 어디로 모시고 가야 할지 모르겠어.", e:"I’m having some clients over to the office this evening, but I don’t really know where to take them to dinner after the meeting."},
            {d:90, s:"교재2", k:"광흥창역 바로 옆에 괜찮은 양고기 집에 모시고 가면 괜찮을 것 같은데. 서울에서 먹어 본 양고기 중에 최고야. 분명 만족하실 거야.", e:"Maybe you could take them out to that nice lamb place right next to Gwangheungchang station. It’s the best lamb I’ve had in Seoul. I’m sure they will love it."},
            {d:90, s:"교재2", k:"Jeff, 발표 자료에서 이 슬라이드는 빼면 어떨까요? 큰 의미가 없는 내용이라서 빼도 큰 상관없을 듯한데.", e:"Jeff, what do you think about taking this slide out of the presentation? It doesn’t seem to add much value."},
            {d:90, s:"교재2", k:"사실 저도 그렇게 느꼈거든요. 빼야 할지 말아야 할지 고민하고 있었어요. 팀장님 말씀대로 할게요.", e:"Actually, I was feeling the same way. I wasn’t sure whether to get rid of it or not. I’ll follow your advice."},
            {d:90, s:"교재3", k:"가끔 방송 직전에 내용이 바뀐다.", e:"Sometimes things change at the last minute."},
            {d:90, s:"교재3", k:"대본이 이미 출력된 상태라서 대본을 수정하거나 이전 내용을 뺄 시간이 없다.", e:"There is no time to change the script or take out the outdated information, and it’s already printed out."},
            {d:90, s:"교재3", k:"내가 할 수 있는 유일한 것은 방송 중에 빼고 읽을 수 있도록 수정 전 내용에 표시해 두는 것이다.", e:"The only thing I can do is highlight the outdated details, so I know to leave them out on the air."},
            {d:90, s:"교재3", k:"다른 사람이 보면 중요한 정보에 표시하는 줄 알 것이다.", e:"To some, it may look like I’m highlighting important information."},
            {d:90, s:"교재3", k:"하지만 사실 반대다. 생방송 분야의 경우, 상황이 빠르게 바뀐다.", e:"But to me, it’s the opposite. In the field of live TV, things can change quickly."},
            {d:90, s:"교재3", k:"순발력 있게 판단하고 말을 바꿔서 할 준비가 되어 있어야 한다.", e:"You have to think fast and be ready to change what you say."},
            {d:90, s:"교재3", k:"이 일을 하면서 예상치 못한 상황에 대처하는 법도 배우게 되었다.", e:"This job has taught me how to handle surprises well."},

            // Day 91
            {d:91, s:"교재1", k:"옷이 (많아서) 옷장 공간을 너무 많이 차지하네. 일부는 버리자.", e:"Our clothes are taking up too much space in the closet. Let’s get rid of some of them."},
            {d:91, s:"교재1", k:"당분간 SNS 좀 쉬어야 할 것 같아. 인스타 보느라 시간을 너무 많이 뺏겨.", e:"I think I will take a break from social media. Scrolling on Instagram is taking up so much time."},
            {d:91, s:"교재1", k:"실내 등반을 시작해 볼까 싶습니다. 새로운 사람을 만날 수 있는 좋은 방법인 것 같아요.", e:"I was thinking of taking up indoor climbing. It seems like a good way to meet new people."},
            {d:91, s:"교재1", k:"내가 탁구에서 형을 진짜 못 이긴다고 생각해? 기꺼이 응해 줄게. 내가 이겨. 한번 보자고!", e:"You don’t think I can beat you in ping-pong? I’ll take you up on that. I’ll win; just wait and see!"},
            {d:91, s:"교재1", k:"새로 산 소파가 원룸 공간을 너무 많이 차지한다.", e:"My new couch takes up too much space in my studio apartment."},
            {d:91, s:"교재1", k:"출퇴근 시간이 너무 많이 걸려서 차를 한 대 사야 할 것 같다.", e:"Commuting to work is taking up too much time, so I need to buy a car."},
            {d:91, s:"교재1", k:"새로운 취미를 같이 시작해 볼까 해서 남편과 저는 골프를 시작했어요.", e:"My husband and I took up golf to try and start a new hobby together."},
            {d:91, s:"교재1", k:"A: 엄마가 너한테 한국 전통 요리를 해 주고 싶대. 어때?", e:"A: My mom said she’d like to make you a traditional Korean meal. What do you think?"},
            {d:91, s:"교재1", k:"B: 우와. 멋질 것 같아. 당연히 초대에 응해야지.", e:"B: Wow, that’d be amazing. I’ll definitely take you up on that."},
            {d:91, s:"교재2", k:"새 회사에서 근무를 시작한 후, 하루의 대부분을 단순한 행정 업무를 하는 데 보내고 있어.", e:"Since starting off with a new company, so much of my day is taken up by mundane tasks."},
            {d:91, s:"교재2", k:"조금 인내심을 가져. 그 회사에서 네가 가장 신입이잖아. 시간이 지나면 널 좀 더 신뢰할 거야. (신뢰하게 되면 좀 더 중요한 일을 맡길 거라는 말)", e:"Just be patient. You’re still the newest employee there. They’ll trust you more with time."},
            {d:91, s:"교재2", k:"뜨개질을 배워 볼까 싶어. 딸아이 가방을 만들어 주고 싶거든.", e:"I was thinking of taking up crochet. I want to make a bag for my daughter."},
            {d:91, s:"교재2", k:"다른 취미를 또 시작한다고? 이미 너무 많은 취미를 가지고 있다고 생각하지 않아?", e:"Taking up another hobby? Don’t you think you have enough on your plate already?"},
            {d:91, s:"교재2", k:"로스앤젤레스에 머물 곳이 필요하면 우리 집에 남는 방이 있어.", e:"If you ever need a place to stay in Los Angeles, I have an extra room."},
            {d:91, s:"교재2", k:"초대 무조건 받아들일게. 내년 여름에 캘리포니아 갈 거거든.", e:"I will definitely take you up on that. I’ll be in California next summer."},
            {d:91, s:"교재3", k:"귀하의 매장을 방문한 건 정말 멋진 경험이었습니다.", e:"Visiting your store was a great experience for me."},
            {d:91, s:"교재3", k:"정말 친절하셨고 질문에 모두 답변해 주셨습니다.", e:"You were extremely helpful and answered all of my questions."},
            {d:91, s:"교재3", k:"매장을 나선 후, 아무것도 사지 않고 시간을 너무 많이 뺏은 것 같아서 미안한 생각이 들었습니다.", e:"After leaving the store, I felt sorry for taking up so much of your time without buying anything."},
            {d:91, s:"교재3", k:"사실 제가 딱 필요한 것이 없었습니다. 저에게 딱 맞는 것이 아닌데 예산을 한참 초과해서 구매하고 싶지는 않았답니다.", e:"Honestly, there was nothing that was exactly what I needed, and I didn’t want to go so far out of my price range for something that wasn’t a perfect fit."},
            {d:91, s:"교재3", k:"그래도 고객 서비스는 정말 훌륭했습니다. 정말 고맙습니다.", e:"The customer service was exemplary, though. I really appreciate that."},
            {d:91, s:"교재3", k:"다음번에 안경이 필요할 경우 다시 방문해서 매니저님을 찾겠습니다.", e:"I’ll come back and ask for you the next time I need glasses."},
            {d:91, s:"교재3", k:"친구와 가족들에게도 귀하의 매장을 추천하겠습니다.", e:"I’ll also be recommending your store to friends and family."},

            // Day 92
            {d:92, s:"교재1", k:"버스 정류장에 쓰레기를 무단으로 버리는 아저씨에게 (제가) 한 소리 했어요.", e:"I told off some guy for littering at the bus stop."},
            {d:92, s:"교재1", k:"그 친구가 다시 한번 널 괴롭히면 형한테 말해. 너 대신 내가 혼내 줄게.", e:"If he ever bothers you again, let me know. I’ll tell him off for you."},
            {d:92, s:"교재1", k:"내가 미안하다고 했잖아! 굳이 밤새 그렇게 뭐라 할 필요는 없잖아.", e:"I said I was sorry! You don’t need to chew me out all night."},
            {d:92, s:"교재1", k:"수업 빠지려면 너 혼자 빠져. 교장 선생님한테 다시 혼나고 싶지 않거든.", e:"You can ditch class without me. I’m not gonna get chewed out by the principal again."},
            {d:92, s:"교재1", k:"제 약혼녀가 저 보고 여자 동료랑 커피 마셨다고 엄청 뭐라고 했어요. 질투심이 많은 사람이에요.", e:"My fiancée told me off for having coffee with a female colleague of mine. She’s the jealous type."},
            {d:92, s:"교재1", k:"담배 피운 것 때문에 고등학생들이 선생님한테 혼나고 있는 거 봤어.", e:"I saw some high school kids getting told off by their teacher for smoking."},
            {d:92, s:"교재1", k:"코치님이 슛을 놓칠 때마다 너무 혼을 내서 농구팀을 그만두었답니다.", e:"I quit the basketball team because my coach would chew me out every time I missed a shot."},
            {d:92, s:"교재2", k:"Mike가 놀이터에서 또 자기 아빠한테 혼나는 거 들었어?", e:"Did you hear Mike getting told off by his dad again at the playground?"},
            {d:92, s:"교재2", k:"응, 불쌍한 Mike. Mike 아빠는 너무 엄격해.", e:"Yeah, poor Mike. His dad is way too strict."},
            {d:92, s:"교재2", k:"이야, 손님을 그렇게 혼내다니 (너도 참) 대단하다. 다른 손님들이 박수를 치더라.", e:"Whoa, I can’t believe you told off that customer. The other people clapped for you."},
            {d:92, s:"교재2", k:"그 무례한 여성분이 선을 넘었잖아. 무례하게 행동해도 괜찮다고 생각하는 손님들에게는 본때를 보여 줘야 해. 이 때문에 매니저님이 나를 혼내도 상관없어.", e:"That rude lady crossed the line. Customers who think they can behave badly and get away with it need to be taught a lesson. I don’t care if the manager chews me out."},
            {d:92, s:"교재2", k:"새로 나온 <배트맨> 영화 보러 가자. 8시에 시작해.", e:"Let’s go watch the new Batman movie. It starts at 8."},
            {d:92, s:"교재2", k:"내가 말했잖아. 내일 아침까지 이 숙제 못 끝내면 선생님이 반 아이들 앞에서 또 나를 혼내실 거야.", e:"I told you, if I don’t get this homework done by tomorrow morning, the teacher is gonna chew me out in front of the whole class again."},
            {d:92, s:"교재3", k:"명절은 가족이 함께하기에 너무 좋은 시간이다. 하지만 상처를 줄 수 있는 원인이기도 하다.", e:"The holidays are great for family time, but they can also be a source of hurt."},
            {d:92, s:"교재3", k:"작년에 나는 아내에게 이번 추석에만 광주에 계신 그녀의 부모님을 뵈러 가는 걸 건너뛰자고 제안했다.", e:"Last year, I suggested to my wife that we skip visiting her parents in Gwangju for Chuseok just once."},
            {d:92, s:"교재3", k:"이 때문에 크게 다투었다. 아내의 가족을 소홀히 한다고 나보고 엄청 뭐라고 했다.", e:"That started a big argument, where she chewed me out for not respecting her side of the family."},
            {d:92, s:"교재3", k:"우리 부모님 댁에는 매번 명절마다 갔다는 점을 일깨워 줬다.", e:"She reminded me that we have never missed a holiday at my parents’ house."},
            {d:92, s:"교재3", k:"내가 괜한 이야기를 했다. 결국 처가댁에 갔다.", e:"I regretted bringing it up, and we ended up going."},
            {d:92, s:"교재3", k:"자기가 원하는 것을 얻었지만, 그럼에도 아내는 별로 탐탁지 않게 생각했을 것이다.", e:"I still don’t think she was happy getting what she wanted."},

            // Day 93
            {d:93, s:"교재1", k:"A(영국인): 커피 잔 좀 채워 줘. 오늘은 카페인이 더 필요해.", e:"A: You can top up my coffee. I need extra caffeine today."},
            {d:93, s:"교재1", k:"B(미국인): top up이라고? top off 말이지?", e:"B: Top up? Do you mean top off?"},
            {d:93, s:"교재1", k:"A(영국인): 아니, 커피 좀 더 달라고 할 때의 top up이라는 말이었는데.", e:"A: No, I mean top up, as in, more coffee please."},
            {d:93, s:"교재1", k:"이 핫 초콜릿에 휘핑크림을 얹으면 더 맛있을 것 같은데.", e:"This hot chocolate would be better if I could top it off with some whipped cream."},
            {d:93, s:"교재1", k:"오늘 아침 출근길에 여자 친구가 문자를 보내서는 헤어지자고 했다. 문자에 답을 하기도 전에 회사에 있는 내 책상이 치워져 버린 걸 발견했고, 회사에서 나를 더 이상 필요로 하지 않는다고 통보받았다. 여자 친구와 직장을 잃은 것도 모자라, 내 차 앞 유리에는 주차 위반 딱지까지 붙어 있었다.", e:"On my way to work this morning, my girlfriend sent me a text saying she wants to break up. Before I could even respond, I saw that my desk was cleaned out, and I was informed that I was no longer needed at the company. I lost my girlfriend and my job, and to top it all off, I found a parking ticket on my windshield."},
            {d:93, s:"교재1", k:"A: 커피가 거의 없구나. 가득 따라 줄까?", e:"A: You’re a bit low on coffee. Want me to top you off[up]?"},
            {d:93, s:"교재1", k:"B: 고맙지만 괜찮아.", e:"B: No, thanks. I’m good."},
            {d:93, s:"교재1", k:"팬케이크에는 신선한 블루베리와 메이플 시럽을 얹어서 먹으면 제일 좋다.", e:"Pancakes are best when topped off with fresh blueberries and maple syrup."},
            {d:93, s:"교재1", k:"나는 다시는 요리하지 않을 것이다. 뜨거운 기름에 화상을 입었고, 양파를 썰다가 손가락이 베일 뻔했는데, 설상가상으로 저녁 요리를 다 끝내지 못해 배고파 죽겠다!", e:"I’ll never cook again. I burned myself with hot oil, I almost cut my finger off trying to chop an onion, and to top it off, I’m starving because I couldn’t even finish cooking my dinner!"},
            {d:93, s:"교재2", k:"브레이크 교체하는 동안 엔진오일도 가득 채워 드렸습니다.", e:"While we were replacing your brakes, we went ahead and topped off your oil as well."},
            {d:93, s:"교재2", k:"너무 고마워요. 감사합니다.", e:"Thanks a lot. I appreciate that."},
            {d:93, s:"교재2", k:"우와, Kelly, 피부가 검게 탔구나. 호주는 어땠어?", e:"Wow, Kelly, you look so tan. How was Australia?"},
            {d:93, s:"교재2", k:"브리즈번 여행은 꿈 같았어. 우선 항공사에서 일등석으로 업그레이드를 해 줬어. 그리고 호텔에 도착했더니 우리를 펜트하우스 스위트룸에 무료로 묵게 해 줬지. 게다가 해변에서 멋진 분들도 만났어. 잊을 수 없는 여행이었어.", e:"Our trip to Brisbane was like a dream. First, the airline upgraded our seats to first class, then when we arrived at the hotel, they put us up in the penthouse suite, free of charge. To top it all off, we met some great people at the beach. It was an unforgettable trip."},
            {d:93, s:"교재2", k:"안녕, Dan. 드디어 금요일이다. 오늘 하루는 어땠어?", e:"Hey, Dan. TGIF. How was your day?"},
            {d:93, s:"교재2", k:"금요일이라 좋다고? 난 아닌데. 교실로 달려가다가 휴대폰을 떨어뜨렸어. 화면에 금이 갔지 뭐야. 설상가상으로 수학 시험을 망쳤단 말이야.", e:"TGIF? Not for me. I was running to class and I dropped my phone. The screen is cracked. To top it all off, I did horrible on that math quiz."},
            {d:93, s:"교재3", k:"오늘 정말 재수가 없다.", e:"I’m really having some horrible luck today."},
            {d:93, s:"교재3", k:"아침에 버스를 놓쳐서 하마터면 회사에 지각할 뻔했다.", e:"I missed my bus in the morning, which would’ve made me late to work."},
            {d:93, s:"교재3", k:"그것도 모자라 설상가상으로 버스를 안 놓치고 타려다가 발목을 삐었다. 이 때문에 병원에 가야 해서 하루 종일 일도 못했다.", e:"That would’ve been bad enough, but to top it off, I twisted my ankle while trying to catch the bus and had to miss a whole day of work because I had to go to the hospital."},
            {d:93, s:"교재3", k:"의사 선생님 말로는 발목을 조금 삐끗한 정도지만 그래도 일주일 동안 목발을 사용해야 한다고 한다.", e:"The doctor says my ankle is only sprained, but that I should use crutches for a week."},
            {d:93, s:"교재3", k:"내일 중요한 회의가 있다는 점이 문제다. 무슨 일이 있었는지 설명하기가 민망하다.", e:"Now my problem is that I have an important meeting tomorrow, and it’s too embarrassing to explain what happened."},
            {d:93, s:"교재3", k:"목발을 하고 걸어 들어가는 것과 목발을 안 하고 절뚝거리며 들어가는 것 중에 뭐가 더 안 좋을지….", e:"I don’t know what’s worse, walking in with crutches, or limping in without them…"},

            // Day 94
            {d:94, s:"교재1", k:"비행기 출발 불과 40분 전에 공항에 도착했기 때문에 걱정을 했어요. 근데 알고보니 비행기가 (이륙이) 지연되었더군요. 그래서 놓치지 않았답니다.", e:"I was worried because I made it to the airport only 40 minutes before my flight, but it turns out the plane was delayed, so I didn’t miss it."},
            {d:94, s:"교재1", k:"해변에 가는 길에 날씨가 걱정되었다. 하지만 막상 도착하니 날씨가 정말 좋았다.", e:"We were worried about the weather on our way to the beach, but it turned out to be a really beautiful day once we got there."},
            {d:94, s:"교재1", k:"그 영화 예상보다 훨씬 괜찮더라. 또 보고 싶어.", e:"The movie turned out to be much better than I expected. I want to see it again."},
            {d:94, s:"교재1", k:"중간고사로 스트레스가 심했는데, 막상 뚜껑을 열어보니 괜찮았다. 생각했던 것 보다 훨씬 쉬웠다.", e:"I was so stressed about my midterm exam, but it actually turned out fine. It was way easier than I thought it would be."},
            {d:94, s:"교재1", k:"행사 준비에 애를 먹었지만 결과는 대성공이었다.", e:"Despite the struggles we had planning, the event turned out really great."},
            {d:94, s:"교재1", k:"면접 결과에 따라 어쩌면 다른 도시로 이사하는 것을 고려해야 할 수도 있어.", e:"Depending on how the job interview turns out, I may have to consider moving to a new city."},
            {d:94, s:"교재1", k:"알고 보니 점화 플러그 중 하나가 고장이 난 거였어. 이것만 교체했더니 엔진이 다시 작동하더라.", e:"It turned out one of my spark plugs was broken. Replacing it was all I needed to get the engine running again."},
            {d:94, s:"교재1", k:"인스타에서 본 이 새로운 레시피를 시도해 봤어. 영상에서는 맛있어 보였는데, 막상 해 보니 맛이 너무 심심하더라고. 레시피대로 했는데, 내가 뭘 잘 못한 건지….", e:"I tried this new recipe I saw on Instagram. It looked so good in the video, but it turned out so bland. I followed the recipe exactly, so I can’t see where I went wrong…"},
            {d:94, s:"교재1", k:"이번 분기 매출 상황에 따라 일부 제품의 생산을 중단해야 할 수도 있습니다.", e:"Depending on how sales turn out this quarter, we may have to discontinue some of our products."},
            {d:94, s:"교재2", k:"Jeff랑 두 번째 데이트는 어땠어? 여전히 좋고 친절해 보였어? 첫 데이트 하고 나서 너 엄청 들떠 보였어.", e:"So how was your second date with Jeff? Did he still seem nice and friendly? You looked so excited after the first date with him."},
            {d:94, s:"교재2", k:"알고 보니 전혀 스윗하지 않았어. 다 연기였어! 결국 전 남자 친구들과 다를 게 없지. 욱, 아무리 좋게 이야기해도 너무 거만하더라고", e:"It turns out he wasn’t that sweet after all. It was all an act! He ended up being just like all of my exes. Ugh, he was way too cocky, to say the least."},
            {d:94, s:"교재2", k:"민수 목소리가 감기 걸린 것 같아서 걱정했는데 알고 보니 숙취였어.", e:"I was worried. Minsu sounded like he was coming down with a cold, but it turns out he was just hung over."},
            {d:94, s:"교재2", k:"하하. 심각한 건 아니라 다행이군. 습관이 되면 안 되는데.", e:"Haha. I’m glad it was nothing serious. I just hope he doesn’t make a habit of it."},
            {d:94, s:"교재2", k:"보통 소설 읽을 때면 결론이 궁금해서 마지막으로 건너뛰거든. 그런데 《불편한 편의점》은 1페이지부터 너무 재미있어서 결론부터 알고 싶지 않더라고.", e:"Whenever I pick up a new novel, I almost always skip to the end to see how the story turns out. But with Inconvenient Convenience Store I was hooked from the first page, and I didn’t want to spoil it for myself."},
            {d:94, s:"교재2", k:"무슨 말인지 너무 잘 알 것 같아. 나도 막 읽었는데, 보통 새 책은 첫 장을 넘어가기 힘든데 이 소설은 하룻밤에 다 읽었어.", e:"I totally get what you mean. I just read it myself, and I normally can’t make it past the first chapter of a new book. I ended up reading it all in one night."},
            {d:94, s:"교재3", k:"저희 할머니가 하셨던 것처럼 칼국수를 만들어 보려고 했는데, 그 맛이 안 났습니다. 뭔가 빠진 느낌이었죠.", e:"I tried to cook kalguksu just like my grandma used to, but it didn’t turn out quite right. Something was missing."},
            {d:94, s:"교재3", k:"그래서 할머니가 만든 칼국수가 맛있는 데에는 레시피에 무언가 특별한 재료가 있다는 생각이 들었습니다.", e:"This made me think that there must be a special ingredient in her recipe that makes her kalguksu so tasty."},
            {d:94, s:"교재3", k:"그래서 전화를 해서 비밀이 뭐냐고 여쭤보았답니다. 그런데 말이에요,", e:"I called and asked what the secret was, but you know what?"},
            {d:94, s:"교재3", k:"알고 보니 그냥 인공 조미료를 많이 넣는 거였어요.", e:"It turned out she just adds lots of artificial seasoning."},
            {d:94, s:"교재3", k:"몹시 실망스러웠지만, 조금 있으니 충동이 생겼어요. 그래도 할머니 레시피를 꼭 시도해 보고 싶더라고요.", e:"That was a big disappointment, but then, a bit later, I got an urge; I still had to try her recipe."},
            {d:94, s:"교재3", k:"인정할 수밖에 없는 것이, 인공 조미료를 많이 넣었음에도 할머니가 만든 칼국수 맛을 당해낼 수는 없었습니다.", e:"Even with all the artificial seasoning, I have to admit, nothing can compare to that taste."},

            // Day 95
            {d:95, s:"교재1", k:"나는 눈을 뜬 뒤에도 누워서 몇 분간 있다가 일어나는 경우가 종종 있다.", e:"After I wake up, I sometimes just lie there for several minutes before finally getting up."},
            {d:95, s:"교재1", k:"한밤중에 두 번 이상 깨서 화장실에 간다면, 전립선에 문제가 있다는 신호일 수 있다.", e:"If you wake up more than twice a night to go to the bathroom, that could indicate a problem with your prostate."},
            {d:95, s:"교재1", k:"일찍 일어나는 건 정말 내 체질이 아니다. 그래서 프리랜서로 일하는 게 너무 행복하다.", e:"Getting up early isn’t really for me. That’s why I’m really happy to work as a freelancer."},
            {d:95, s:"교재1", k:"영화관에서 계속 잠이 들었다 깼다 해서 줄거리를 전혀 쫓아가지 못했다.", e:"I kept falling asleep and waking up in the movie, so I couldn’t follow the plot at all."},
            {d:95, s:"교재1", k:"중학교 때 친구 중 한 명이 눈을 뜬 채로 잠들 수 있는 놀라운 기술이 있었다. 어느 날 담임선생님이 마침내 알아채시고 친구를 깨우셨다.", e:"When I was in middle school, one of my friends had an amazing skill ‒ she could fall asleep with her eyes open. My homeroom teacher finally caught her one day and woke her up."},
            {d:95, s:"교재1", k:"Nicholas, 지금 당장 안 일어나면 학교에 늦어!", e:"Nicholas, if you don’t get up right this instant, you’re going to be late for school!"},
            {d:95, s:"교재1", k:"자, 여러분, 2부 수업을 시작하기 전에 다들 자리에서 일어나서 다리 스트레칭을 합시다.", e:"Alright, class, let’s get up and stretch our legs before we start the second half of today’s lesson."},
            {d:95, s:"교재2", k:"난 아침에 너무 힘들어. 알람이 한 번 울려서는 절대로 눈이 안 떠져.", e:"I have such a hard time in the mornings; I can never wake up on the first alarm."},
            {d:95, s:"교재2", k:"알람을 방의 (누워서 자는 쪽) 반대편에 둬 봤어? 그러면 알람을 끄기 위해서라도 어쩔 수 없이 침대 밖으로 나와야 되거든.", e:"Have you tried putting your alarm across the room? It forces you to get out of bed to turn it off."},
            {d:95, s:"교재2", k:"우리 집 고양이는 매일 아침 5시에 나를 깨우는 성가신 버릇이 있어. 하지만 내가 5시에 일어나는 경우는 드물어. 잠깐 고양이를 토닥여 주고는 다시 잠들지.", e:"My cat has this annoying habit of waking me up at 5 a.m. every day, but I rarely get up then. I usually just pet her a little while and then go back to sleep."},
            {d:95, s:"교재2", k:"재미있네. 우리 집 개는 정반대거든. 일찍 눈을 떠도 내가 완전히 침대 밖을 나와서 산책을 시켜 줄 준비가 되었는지 볼 때까지 조용히 있어.", e:"That’s funny; my dog is the opposite. He wakes up early but stays quiet until he sees that I’m fully out of bed and ready to take him for a walk."},
            {d:95, s:"교재2", k:"화장실 가시려고요? 제가 일어날까요?", e:"Do you need to go to the bathroom? Would you like me to get up?"},
            {d:95, s:"교재2", k:"아닙니다. 괜찮습니다. 괜찮으시다면 제가 넘어서(타넘고) 가면 됩니다.", e:"No, thank you. That’s okay. I’ll just step over you if that’s alright."},
            {d:95, s:"교재3", k:"30대 때에는 잠자리에 드는 데 전혀 문제가 없었다.", e:"In my thirties, I had no trouble at all getting to sleep."},
            {d:95, s:"교재3", k:"게다가 한밤중에 단 한 번도 깨지 않았다.", e:"Plus, I never woke up even once in the middle of the night."},
            {d:95, s:"교재3", k:"이제 40대가 되다 보니 밤에 서너 번은 꼭 깬다.", e:"Now that I’m in my forties, though, I’m always waking up ‒ like three or four times a night."},
            {d:95, s:"교재3", k:"수면에 정말 방해가 된다.", e:"It really disrupts my sleep."},
            {d:95, s:"교재3", k:"나이 탓인지, 사업체 운영으로 인한 압박 때문인지는 모르겠다. 어쩌면 둘 다일지도.", e:"I don’t know if it has to do with my age or the pressure of running my own business. Maybe it’s both."},
            {d:95, s:"교재3", k:"아침에 일어나는 건 너무 힘들다.", e:"Getting up in the morning has become a pain."},
            {d:95, s:"교재3", k:"완전히 정신이 들고 출근할 준비가 되려면, 한 시간은 족히 걸리고 커피도 몇 잔 마셔야 한다.", e:"It takes a good hour and several cups of coffee before I finally start to feel awake and ready to work."},

            // Day 96
            {d:96, s:"교재1", k:"남편 조부모님을 찾아뵙고 나서, 남편은 자신이 자란 옛날 동네를 데리고 다니며 구경을 시켜 줬습니다. 남편이 다닌 학교를 보고, 남편이 가장 좋아했던 예전 과자 가게를 볼 수 있어서 너무 좋았습니다.", e:"After we visited my husband’s grandparents, he walked me through his old neighborhood. It was cool to see the schools and his favorite old snack shop."},
            {d:96, s:"교재1", k:"프린터 사용방법을 전혀 모르겠더라고요. 그래서 매니저님이 자세히 설명해 주셨습니다.", e:"I couldn’t figure out how to work the printer, so my manager walked me through it."},
            {d:96, s:"교재1", k:"우리끼리 전시 작품을 이해하려 해 봤자 소용없어. 3시까지 여기서 기다리자. 그러면 가이드가 우리를 안내해 줄 거야.", e:"It’s kind of a waste to try and understand the exhibition on our own. Let’s wait here until 3, and then a guide will walk us through."},
            {d:96, s:"교재1", k:"부동산 중개인이 서초에 있는 아파트 몇 채를 보여 줬습니다. 하지만 제 취향은 아니었어요.", e:"The realtor walked me through several apartments in Seocho, but they weren’t my cup of tea."},
            {d:96, s:"교재1", k:"다음은 Steve 차례입니다. 그가 새롭게 리뉴얼된 홈페이지를 자세히 설명해 줄 겁니다.", e:"Next up is Steve. He’s going to walk us through the new homepage redesign."},
            {d:96, s:"교재1", k:"컴퓨터를 잘 알지 못해도 괜찮아. 내가 소프트웨어 업데이트를 자세히 설명해 줄게.", e:"It’s okay if you are not computer-savvy. I will walk you through the software update."},
            {d:96, s:"교재2", k:"여기서 도서관에 어떻게 가는지 설명 좀 해 주시겠어요?", e:"Can you show me how to get to the library from here?"},
            {d:96, s:"교재2", k:"당연하죠. 저를 따라오세요. 제가 캠퍼스를 안내해 드릴게요. 얼마 안 가면 도서관이 나올 겁니다.", e:"Sure. Come with me. I’ll walk you through the campus, and we’ll get to the library in no time."},
            {d:96, s:"교재2", k:"뭐가 이렇게 지저분해! 잘 모르겠으면 유튜브 설명 영상을 찾아보면 되잖아.", e:"Look at this mess! If you are stuck, just find a YouTube tutorial."},
            {d:96, s:"교재2", k:"아, 맞아. 정말 바보 같네! 유튜브 영상에서 자세히 알려 줄 거라고는 생각도 못 했어.", e:"Ah, you’re right. I feel so silly! It never occurred to me that a YouTube video could walk me through it."},
            {d:96, s:"교재2", k:"의류 관리기 새 제품 구매하신 것을 축하드립니다! 댁에 사람을 보내서 기능과 사양을 자세히 설명드리도록 할까요?", e:"Congratulations on your purchase of a new steam closet! Would you like us to send somebody to your home to walk you through all the features?"},
            {d:96, s:"교재2", k:"아니요, 괜찮습니다. 제가 알아서 할 수 있을 것 같습니다.", e:"No, thanks. I think I can figure them out on my own."},
            {d:96, s:"교재3", k:"약을 복용하기 전에 저희 약사들 중 한 분과 상의하는 것이 중요합니다.", e:"It’s important to talk with one of our pharmacists before you start taking your medication."},
            {d:96, s:"교재3", k:"그분들이 복용량과 복용 시간 등등 세부적인 내용을 자세히 설명드릴 겁니다.", e:"They can help walk you through the details, like how much to take and when."},
            {d:96, s:"교재3", k:"궁금한 점 있으면 뭐든 그분들에게 물어보면 됩니다.", e:"You can also ask them any questions you might have."},
            {d:96, s:"교재3", k:"약사분들은 환자 여러분이 회복 과정에서 더 주도적으로 느끼도록 도와드립니다.", e:"They’re here to help you feel more in charge of your recovery."},
            {d:96, s:"교재3", k:"이러한 대화는 여러분의 건강을 관리하는 방법에 큰 변화를 가져올 것입니다.", e:"This chat can make a big difference in how you manage your health."},
            {d:96, s:"교재3", k:"기억하세요. 시간을 내어 여러분이 복용하는 약에 대해 잘 이해하면 회복이 빨라질 수 있으며 회복 과정도 무리 없이 진행될 수 있습니다.", e:"Remember, taking the time to understand your medication can help speed up your recovery and make the whole process smoother."},

            // Day 97
            {d:97, s:"교재1", k:"새로운 기업에 지원하기 시작하려면 이번 주말에 포트폴리오 작업을 해야 해.", e:"I need to work on my portfolio this weekend so I can start applying to new companies."},
            {d:97, s:"교재1", k:"A: 요즘 무엇 때문에 바쁜 거야?", e:"A: What’s keeping you busy these days?"},
            {d:97, s:"교재1", k:"B: 이 춤 동작 연습하는 게 생각보다 훨씬 더 어려워. 나아지고는 있지만 연습이 더 필요해.", e:"B: Working on this dance move is way harder than I thought. I’m getting better, but I still need more practice."},
            {d:97, s:"교재1", k:"너무 빨리 먹는구나. 식탁 예절 연습이 필요하겠어.", e:"You eat so fast. You need to work on your table manners."},
            {d:97, s:"교재1", k:"나랑 경기해 줘서 고마워. 참고로 나 서브 연습 안 한 지 한참 됐어.", e:"Thanks for playing with me. Just letting you know, I haven’t worked on my serve in a while."},
            {d:97, s:"교재1", k:"제가 새 책 쓰느라 엄청 바쁩니다.", e:"I am super busy working on my new book."},
            {d:97, s:"교재1", k:"새 앱을 개발하고 있는데요. 제가 대단한 프로그래머는 아니라서 AI가 큰 도움이 됩니다.", e:"I have been working on a new app. I’m not much of a programmer, so AI is a big help."},
            {d:97, s:"교재2", k:"오늘은 하체 운동 하는 날이네요! 준비되셨을까요? 정말 하체를 좀 더 키워야 합니다.", e:"It’s leg day! Do you think you’re ready? You really need to work on your legs more."},
            {d:97, s:"교재2", k:"하아, 네. 근데 데드리프트는 좀 건너뛰면 안 될까요? 며칠 전에 운동한 것 때문에 아직 등이 아프네요.", e:"Ugh, okay. Is there any way I could skip deadlifts, though? My back is still sore from our workout a few days ago."},
            {d:97, s:"교재2", k:"내년에 호주로 한 달간 여행을 갈 생각이야. 그 전에 영어 공부 좀 해야 돼!", e:"I’m planning a one-month trip to Australia next year. I really need to work on my English before then!"},
            {d:97, s:"교재2", k:"진심이니? 말도 안 돼. 나보다 영어를 열 배는 더 잘하면서!", e:"Are you serious? You’re joking, right? Your English is already ten times better than mine!"},
            {d:97, s:"교재2", k:"이번 주 과제로 발음 연습을 더 많이 해 주시길 부탁드립니다. 특히 ‘R’ 발음이요.", e:"For homework this week, I’d like you to work more on your pronunciation. Especially the letter “R.”"},
            {d:97, s:"교재2", k:"네, 감사합니다. 지난주에 호주 친구인 John이 ‘low’를 ‘row’로 알아들어서 정말 민망했답니다. 연습을 더 해야 할 동기부여가 됐어요.", e:"Okay, thank you. I was pretty embarrassed last week when my Australian friend, John, mistook “low” for “row.” It definitely gave me the motivation to practice more."},
            {d:97, s:"교재3", k:"대학 졸업 후 영상 편집자로 일해 오면서 (필요한) 기술을 익히고, 유명인을 만나고, 인맥을 넓혀 왔다.", e:"I’ve been a film editor since college, gaining skills, meeting celebrities, and expanding my network."},
            {d:97, s:"교재3", k:"내가 작업한 프로그램을 TV에서 보는 건 보람 있다. 하지만 TV 프로그램 작업에는 단점도 있다.", e:"Seeing my shows on TV is fulfilling, but working on a TV show comes with some downsides, too."},
            {d:97, s:"교재3", k:"압박이 상당하고 엄격한 마감 시간은 무척 벅차다.", e:"It’s high-pressure, and the strict deadlines can be overwhelming."},
            {d:97, s:"교재3", k:"가끔씩은 이 직업이 나랑 맞나 하는 의구심이 들기도 해서 그만둘까 싶기도 하다.", e:"Sometimes, I doubt if this career is right for me, and I consider quitting."},
            {d:97, s:"교재3", k:"하지만 내 스펙과 경력으로 더 나은 직장을 찾는 것은 어려울지도 모른다.", e:"However, with my credentials and background, finding a better job might be tough."},
            {d:97, s:"교재3", k:"그래서 그냥 참고 지금의 내 상황을 최대한 즐기기로 했다.", e:"For now, I’ve decided to stick it out and make the best of my situation."},

            // Day 98
            {d:98, s:"교재1", k:"A: David, 연구 조사는 어떻게 되어 가고 있나요? 우리 발표 자료를 취합하려고 하는데, 데이터가 더 필요합니다.", e:"A: David, how is the research coming along? I want to start putting our presentation together, but I need more data."},
            {d:98, s:"교재1", k:"B: 지금 하고 있어요. 오늘 퇴근 전까지 보내 둘게요.", e:"B: I’m working on it now. I’ll have it sent to you by the end of the day."},
            {d:98, s:"교재1", k:"Angela는 헬스장 가는 걸 좋아하지 않는다. 벌써 한 달째 시도는 하고 있지만 아직도 마음이 내키지 않아 한다. 그래서 우리 PT 선생님에게 Angela가 헬스장에 오도록 조금 더 설득해 보라고 했다.", e:"Angela doesn’t like coming to the gym. She has tried it for a month now, but still isn’t motivated to go. I asked our personal trainer to work on her a little more."},
            {d:98, s:"교재1", k:"너는 매사에 거절을 못하는구나. 그러니까 스트레스가 심하고 늘 바쁜 거야. 거절하는 연습이 필요해.", e:"You always say “Yes” to everything. That’s why you’re so stressed and busy all the time. You need to work on saying no."},
            {d:98, s:"교재1", k:"A: 여행을 위해 호텔 예약은 마무리한 거야?", e:"A: Did you finish booking hotels for our trip?"},
            {d:98, s:"교재1", k:"B: 하고 있어. 알다시피 조건이 제일 좋은 곳을 찾으려니 시간이 좀 걸리네.", e:"B: I’m working on it. It takes a while to find the best deals, you know."},
            {d:98, s:"교재1", k:"Greg과 Sally가 너무 잘 어울릴 것 같아. 그런데 Sally가 소개팅이라면 좀 신물이 났어. 그래도 설득 중이야.", e:"I think Greg and Sally would be perfect for each other, but Sally is a bit sick and tired of blind dates. I’m still working on her, though."},
            {d:98, s:"교재1", k:"Samantha는 아무 잘못 없었어. 네가 좀 덜 까다롭게 굴도록 노력해야지!", e:"There was nothing wrong with Samantha. You need to work on being less picky!"},
            {d:98, s:"교재2", k:"Dani, 언제까지 대본 보내 줄 수 있겠나? 발표가 3일 앞으로 다가왔어.", e:"Dani, when can you get that script to me? The presentation is only three days away."},
            {d:98, s:"교재2", k:"지금 작성하고 있는데 생각보다 훨씬 오래 걸리네요. 최대한 빨리 보내 드릴게요. 준비할 시간 되실 거예요. 걱정 마세요.", e:"I’m working on it. It’s taking longer than expected, but I’ll get it to you soon. You’ll have time to prepare; don’t worry."},
            {d:98, s:"교재2", k:"제발 음식 가지고 좀 투덜거리지 마. 좀 덜 까다롭게 굴도록 노력해 봐. 모두가 네 엄마처럼 요리를 잘하는 건 아니라는 걸 기억해.", e:"You shouldn’t complain about every meal. You need to work on not being so picky. Remember, not everyone can cook like your mom did."},
            {d:98, s:"교재2", k:"내가 뭐 입이 까다롭다거나 그런 건 아니야. 아내가 해 주는 음식이 좀 심심해서 적응이 안 되는 것뿐이야.", e:"It’s not that I’m picky. I just find the food my wife makes a bit bland, and I’m having a hard time getting used to it."},
            {d:98, s:"교재2", k:"Rebecca, 성공하려면 팀과 잘 어울리는 것이 중요해요. 팀 플레이어(개인보다는 조직을 우선순위에 두는 사람)가 되도록 더 노력해야 합니다.", e:"Rebecca, fitting in with the team is important for success. You should work on being more of a team player."},
            {d:98, s:"교재2", k:"무슨 말씀인지는 알겠습니다. 하지만 모두가 각자의 장점이 있고, 가끔씩 의견이 안 맞을 수도 있잖아요.", e:"I get what you’re saying, but we all have our own strengths, and so sometimes it’s hard to see eye to eye."},
            {d:98, s:"교재3", k:"고등학교 때 3년간 일본어 수업을 들었다. 하지만 졸업 후 손을 놓았다.", e:"I took Japanese for three years in high school, but I haven’t kept up with it since I graduated."},
            {d:98, s:"교재3", k:"이력서에 일본어가 유창하다고 적기 위해서는 일본어 공부를 다시 해야 한다.", e:"I need to work on it again if I want to say that I’m fluent on job applications."},
            {d:98, s:"교재3", k:"그나저나 일본어 배우는 게 영어보다는 쉬운 듯하다.", e:"Learning Japanese seems easier than English, by the way."},
            {d:98, s:"교재3", k:"단어 순서와 문장 구조 때문인 것 같기도 하다.", e:"It might have something to do with the word order and sentence structure."},
            {d:98, s:"교재3", k:"하긴 한국어와 일본어의 언어적 뿌리가 비슷하지 않나.", e:"Korean and Japanese do share similar linguistic roots, after all."},
            {d:98, s:"교재3", k:"외국어 이야기가 나왔으니 말인데 이중언어 구사자들이 부럽다.", e:"Speaking of foreign languages, I am envious of people who grew up bilingual."},
            {d:98, s:"교재3", k:"영어를 배우기 위해 내가 한 고생을 할 필요가 없었을 테니 말이다.", e:"They didn’t even have to go through what I had to go through to learn English."},
            {d:98, s:"교재3", k:"영어 공부에 이렇게 오랜 시간을 투자하고도 아직도 내가 하고자 하는 말을 유창하게 전달하지 못하다니…. 안타깝다!", e:"After all these years of learning English, I am still having trouble getting my message across fluently… What a shame!"},

            // Day 99
            {d:99, s:"교재1", k:"네가 프리랜서로 전향하기로 한 뒤에 상황이 잘 풀려서 다행이다.", e:"I’m glad things worked out for you after you decided to switch to freelancing."},
            {d:99, s:"교재1", k:"A: 예전 동료랑 아직 같이 사는 거야?", e:"A: Are you still rooming together with your old colleague?"},
            {d:99, s:"교재1", k:"B: 아니. 생활 스타일이 너무 달라서 안 맞더라고.", e:"B: No. We had completely different lifestyles, so it didn’t work out."},
            {d:99, s:"교재1", k:"일이 잘 안 풀릴 경우를 대비해 B안을 가지고 있는 건 중요하다.", e:"It is important to have a plan B in case things don’t work out."},
            {d:99, s:"교재1", k:"대학교 1학년 때 기타를 배워 보려 시도했는데, 잘 안되더라고요. 재미가 잘 안 붙었어요.", e:"I tried to learn guitar when I was in my first year of college, but it didn’t work out – I just couldn’t get into it."},
            {d:99, s:"교재1", k:"그녀를 위해 깜짝파티를 해 주려고 했지만, 다들 바빠서 잘 안되었어요.", e:"We wanted to plan a surprise party for her, but with everyone’s busy schedules, it just didn’t work out."},
            {d:99, s:"교재2", k:"혹시 민준이랑 다시 만나는 거 생각해 봤어?", e:"Have you ever thought about getting back together with Minjun?"},
            {d:99, s:"교재2", k:"생각은 해 봤는데, 내 경험으로는 장거리 연애는 힘들어.", e:"I have thought about it, but in my experience, long-distance relationships just don’t work out."},
            {d:99, s:"교재2", k:"호텔 측에서 그러는데 실수로 객실 예약을 너무 많이 받았다네. 그래도 방을 마련해 줄 수 있을지도 모르지.", e:"The hotel says they accidentally overbooked their rooms, but maybe they can still find something for me."},
            {d:99, s:"교재2", k:"알겠어. 정 안 되면 우리 집에 와서 자.", e:"Got it. If things don’t work out, you could always crash on my couch."},
            {d:99, s:"교재2", k:"Terry, <뉴욕타임스> 결제에 문제가 있었던 거 기억하지? 알고 보니 실수로 이중 결제가 됐더라고. 다행히 오늘 아침에 돈이 다시 들어오긴 했어.", e:"Terry, remember my payment issue with the New York Times? Turns out I paid twice by mistake. Luckily, they wired the money back this morning."},
            {d:99, s:"교재2", k:"다행이다! 그쪽에서 빨리 해결을 해서 큰 번거로움 없이 돈을 돌려받았다니 다행이야.", e:"That’s a relief! It’s good to hear they worked it out quickly and you got your money back without much hassle."},
            {d:99, s:"교재3", k:"지난주에 리바이스 501s를 구매한 고객에게서 안 좋은 후기를 받았습니다.", e:"Last week, I got a negative review from a buyer who purchased Levi’s 501s."},
            {d:99, s:"교재3", k:"“슬림핏 청바지이고 제 사이즈가 맞는데 옷이 안 맞아요.”라는 내용이었어요.", e:"The review said, “These were a slim fit and they didn’t fit me even though they were my size.”"},
            {d:99, s:"교재3", k:"그래서 이분에게 연락을 드려서 후기를 좀 수정해 달라고 했습니다.", e:"I contacted him to try and get him to change the review."},
            {d:99, s:"교재3", k:"501s 모델은 ‘슬림핏’이 안 나오고, 일반적인 일자형 핏만 나온다고 정중하게 알려 드리고는 혹시 태그를 잘못 읽었는지 물었습니다.", e:"I politely let him know that 501s don’t come in “slim fit,” only regular straight leg, and asked if he read the tag wrong."},
            {d:99, s:"교재3", k:"그랬더니 놀랍게도 실수를 인정하고 5점을 주었답니다!", e:"To my surprise, he admitted he made a mistake and gave me five stars!"},
            {d:99, s:"교재3", k:"잘됐네요. 일이 잘 해결되어서 다행입니다! 고객에게 연락을 해서 오해를 푼 건 잘한 일이에요.", e:"Nice. Glad it worked out! Good call on reaching out and clearing up the confusion."},
            {d:99, s:"교재3", k:"때로는 간단한 대화로도 문제를 해결할 수 있어 좋습니다.", e:"It’s good to see that a simple chat can turn things around sometimes."},

            // Day 100
            {d:100, s:"교재1", k:"죄송한데 기차 안 놓치려면 지금 가 봐야 할 것 같아요. 일정 조율은 카톡으로 하면 어떨까 합니다. 괜찮으신 거죠?", e:"I’m afraid I need to go now if I want to catch my train. Maybe we can work this out over KakaoTalk. Would that be alright?"},
            {d:100, s:"교재1", k:"민수가 한 이성에게 정착하는 데 대한 두려움이 있어서 그 둘은 힘든 시간을 보내고 있어. 하지만 (둘 간의) 관계를 잘 풀어 가려고 노력 중이야.", e:"They’re going through some ups and downs, especially with Minsu’s fear of settling down, but they’re really trying to work it out."},
            {d:100, s:"교재1", k:"A: 안녕, Aaron. 면접은 어땠어?", e:"A: Hey, Aaron. How was the interview?"},
            {d:100, s:"교재1", k:"B: 그쪽에서 일자리를 제안했어. 좋은 소식이지. 그런데 다음 주에 출입국 관리소에 가 봐야 해. 내 비자 문제를 해결할 수 있는지 알아봐야 하거든.", e:"B: They offered me the job, which is great news, but we need to go to the immigration office next week to see if we can work out my visa."},
            {d:100, s:"교재1", k:"지금은 더 이상 프로젝트 세부 내용에 대한 논의는 힘들 것 같습니다. 다음 주에 만날 때 조율해 보면 어떨까 합니다.", e:"I’m afraid I can’t really discuss the project details any further right now. Maybe we can work things out when we meet next week."},
            {d:100, s:"교재1", k:"제가 가능한 시간은 다음과 같습니다. 수요일, 목요일 오전, 그리고 금요일 저녁입니다. 하지만 다음 주 월요일에 만날 때 조율해 볼 수 있을 것 같습니다.", e:"My availability is as follows: Wednesday, Thursday mornings and Friday evenings. But maybe we could work that out when we meet next Monday."},
            {d:100, s:"교재1", k:"미나와 민수가 현재 상담을 받고 있어. 드디어 둘의 문제를 해결하기 위해 노력하고 있다니 다행이야.", e:"Mina and Minsu are seeing a counselor. I’m happy to hear that they’re finally trying to work out their issues."},
            {d:100, s:"교재1", k:"그룹 프로젝트가 처음에는 불가능해 보였다. 하지만 선생님이 좀 더 도와주시고, 몇 번 밤늦게까지 작업을 한 덕분에, 제때 완수할 수 있는 방법을 찾을 수 있었다.", e:"The group project seemed impossible at first, but with some extra help from the teacher and a few late nights, we were able to work out a way to complete it on time."},
            {d:100, s:"교재1", k:"전반적인 조건에는 합의를 했지만 유급휴가, 근무시간 등 세부 내용은 아직 조율이 필요한 상황입니다.", e:"We have agreed on the overall conditions, but there are still some details that need to be worked out, like paid leave and working hours."},
            {d:100, s:"교재2", k:"남자 친구가 신뢰를 저버렸어요. 하지만 갈등을 해결하고 이겨 내려고 노력 중입니다. 이 사람을 다시 믿기가 정말 어렵네요.", e:"My boyfriend broke my trust, but we are trying to work it out and move past it. It’s so hard to feel confident in him again."},
            {d:100, s:"교재2", k:"힘드시겠네요. 남자 친구와 계속 사귀는 게 의미가 있는지 생각해 보셨나요? 신뢰가 깨지면 그냥 보내 주는 게 좋을 때도 있습니다.", e:"That sounds tough. Have you thought about whether it’s really worth staying together? Sometimes it’s okay to let the guy go once trust is broken."},
            {d:100, s:"교재2", k:"이런! 차가 왜 이래? 괜찮은 거야?", e:"Oh my gosh! What happened to your car? Are you okay?"},
            {d:100, s:"교재2", k:"괜찮아. 집 근처 교차로에서 사고가 났어. 보험회사에 관련 정보 다 보냈고, 이제 과실 비율을 결정할 거야. 근데 이 점(과실 비율)이 가장 많이 신경 쓰이네.", e:"I’m okay. I got into an accident at the intersection near my house. We sent all the information to the insurance companies, and now they are going to work out who was more at fault. That’s what I’m worried about the most."},
            {d:100, s:"교재2", k:"김민재 선수, 우선 뮌헨과의 계약을 축하합니다. 이런 명문 팀에 합류하게 된 기분이 어떤가요?", e:"Minjae Kim, I’d like to congratulate you on your contract with Munich(München). How do you feel about joining this highly-regarded team?"},
            {d:100, s:"교재2", k:"아직 실감이 나지 않습니다. 물론 아직 조율해야 할 세부 내용이 있지만요. 하루라도 빨리 이 팀에 합류하고 싶습니다.", e:"It still doesn’t feel real. Of course, there are still some details that need to be worked out. Joining this team can’t come soon enough."},
            {d:100, s:"교재3", k:"예전에는 강남에 있는 오토바이 대여점에서 (오토바이를) 대여했다. 그런데 이 업체가 최근에 가격을 올렸다.", e:"I used to rent a bike from a shop in Gangnam, but they’ve just raised their prices."},
            {d:100, s:"교재3", k:"오토바이를 하루 빌리는 데 10만 원 이상은 쓰고 싶지 않다.", e:"I don’t want to pay over 100,000 won per day renting a bike."},
            {d:100, s:"교재3", k:"그래서 Marisa라는 분이 오토바이 관련 페이스북 그룹에 (스쿠터 판매 관련) 광고 글을 올린 것을 본 건 정말 다행이다.", e:"That’s why I’m glad I found Marisa advertising on a motorcycle Facebook group."},
            {d:100, s:"교재3", k:"빨간색 빈티지 베스파를 가지고 있었는데, 관리가 완벽했으며 특별한 매력이 있었다.", e:"She had this red vintage Vespa, perfectly maintained and with some unique charm."},
            {d:100, s:"교재3", k:"그녀는 스쿠터를 판매한다는 광고를 올렸는데, 그것도 우리 동네였다. 연락을 해 봤고, 상당히 괜찮은 가격에 합의했다.", e:"She advertised that she was selling her scooter, in my neighborhood even, and when I contacted her, we worked out a very reasonable price."}
        ];

        allData.forEach((item, index) => item.id = index + 1);
        const sourceMap = { 
            "교재1": "Model examples", 
            "교재2": "Small talk", 
            "교재3": "Cases in point", 
            "교재4": "Further studies" 
        };

        let selectedWeekMode = 'w16';
        let quizPool = [];
        let currentIndex = 0;
        let starredIds = new Set();

        function selectWeek(mode) {
            selectedWeekMode = mode;
            document.querySelectorAll('.week-tab').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(`tab-${mode}`).classList.add('active-tab');
            
            const titleEl = document.getElementById('selected-week-title');
            const titles = {
                'w16': 'DAY 76-80', 'w17': 'DAY 81-85', 'w18': 'DAY 86-90',
                'w19': 'DAY 91-95', 'w20': 'DAY 96-100', 'total': 'DAY 76-100 전체'
            };
            titleEl.innerText = titles[mode];
        }

        function unlockAudio() {
            if ('speechSynthesis' in window) {
                const msg = new SpeechSynthesisUtterance("");
                window.speechSynthesis.speak(msg);
            }
        }

        function startQuiz(difficulty) {
            unlockAudio();
            let filtered = [];
            
            if (selectedWeekMode === 'total') {
                filtered = allData.filter(item => item.d >= 76 && item.d <= 100);
            } else {
                // w16: 76-80, w17: 81-85 ...
                const weekNum = parseInt(selectedWeekMode.replace('w', ''));
                const startDay = (weekNum - 16) * 5 + 76;
                const endDay = startDay + 4;
                filtered = allData.filter(item => item.d >= startDay && item.d <= endDay);
            }

            if (difficulty === 'mild') {
                // 순한맛: 교재1만
                filtered = filtered.filter(item => item.s === '교재1');
            } else if (difficulty === 'spicy') {
                // 매운맛: 교재 1, 2, 3 전체
                filtered = filtered.filter(item => item.s === '교재1' || item.s === '교재2' || item.s === '교재3');
            }

            if (filtered.length === 0) {
                alert("해당 범위에 데이터가 없습니다.");
                return;
            }

            quizPool = filtered.sort(() => Math.random() - 0.5).slice(0, 10);
            currentIndex = 0;
            starredIds.clear();
            
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            showQuestion();
        }

        function showQuestion() {
            const item = quizPool[currentIndex];
            const card = document.getElementById('card-container');
            card.classList.remove('flipped');
            document.getElementById('next-btn').classList.add('hidden');
            document.getElementById('star-btn').classList.remove('star-checked');
            
            document.getElementById('q-korean').innerText = item.k;
            document.getElementById('q-english').innerText = item.e;
            document.getElementById('q-info').innerText = `Day ${item.d} - ${sourceMap[item.s]}`;
            document.getElementById('progress-text').innerText = `${currentIndex + 1} / ${quizPool.length}`;
            
            if (starredIds.has(item.id)) document.getElementById('star-btn').classList.add('star-checked');
        }

        function flipCard() {
            document.getElementById('card-container').classList.add('flipped');
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function toggleStar(e) {
            e.stopPropagation();
            const item = quizPool[currentIndex];
            const starBtn = document.getElementById('star-btn');
            if (starredIds.has(item.id)) {
                starredIds.delete(item.id);
                starBtn.classList.remove('star-checked');
            } else {
                starredIds.add(item.id);
                starBtn.classList.add('star-checked');
            }
        }

        function playTTS() {
            const text = document.getElementById('q-english').innerText;
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const msg = new SpeechSynthesisUtterance(text);
                msg.lang = 'en-US';
                msg.rate = 0.9;
                window.speechSynthesis.speak(msg);
            }
        }

        function nextQuestion() {
            currentIndex++;
            if (currentIndex < quizPool.length) showQuestion();
            else showResults();
        }

        function showResults() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            const listEl = document.getElementById('starred-list');
            listEl.innerHTML = '<p class="text-[10px] text-slate-400 font-black mb-4 uppercase tracking-widest text-center">⭐ 체크한 문장 다시보기</p>';
            
            const starredItems = quizPool.filter(item => starredIds.has(item.id));
            if (starredItems.length === 0) {
                listEl.innerHTML += '<p class="text-sm text-slate-400 py-12 text-center font-bold">오답이 없습니다! 완벽해요.</p>';
            } else {
                starredItems.forEach(item => {
                    const div = document.createElement('div');
                    div.className = "p-5 bg-indigo-50 rounded-2xl border border-indigo-100 mb-3";
                    div.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-[9px] bg-indigo-500 text-white px-1.5 py-0.5 rounded font-black">DAY ${item.d}</span>
                            <span class="text-[9px] text-indigo-400 font-bold italic">${sourceMap[item.s]}</span>
                        </div>
                        <p class="text-xs text-slate-500 mb-1">${item.k}</p>
                        <p class="text-sm font-black text-indigo-900">${item.e}</p>
                    `;
                    listEl.appendChild(div);
                });
            }
        }
    </script>
</body>
</html>
