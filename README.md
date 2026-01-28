<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>10-Day Speed Quiz: English Mastery</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Pretendard:wght@400;600;700&display=swap');
        body { font-family: 'Pretendard', sans-serif; background-color: #f1f5f9; }
        .animate-bounceIn { animation: bounceIn 0.5s; }
        @keyframes bounceIn {
            0% { transform: scale(0.9); opacity: 0; }
            70% { transform: scale(1.05); }
            100% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        const RAW_DATA = {
            // --- 1. 영어회화 (Day 50~100) ---
            conversation: [
                { id: 1, kr: "여기서는 원래 그래요.", en: "That’s just how things work here." },
                { id: 2, kr: "골프를 더 잘 치고 싶어요.", en: "I want to get better at golf." },
                { id: 3, kr: "바빠서 운동 할 짬이 안 나네요.", en: "I can’t seem to find (the) time to exercise." },
                { id: 4, kr: "다이소는 가격을 생각하면 꽤 좋은 물건들을 판다.", en: "Daiso has pretty good products for its low prices" },
                { id: 5, kr: "점심 먹으면서 그동안 못했던 이야기하자.", en: "Let’s catch up over lunch." },
                { id: 6, kr: "출근 전에 잠깐 우리 집 들러서 커피 한잔하고 가.", en: "Swing by my place for coffee before work." },
                { id: 7, kr: "죄송한데 선약이 있습니다.", en: "I’m afraid I already have plans." },
                { id: 8, kr: "요새 돈이 좀 궁해.", en: "Money is a bit tight right now." },
                { id: 9, kr: "근무 시간은 어때?", en: "What are the hours like?" },
                { id: 10, kr: "회사에 일이 너무 많긴 한데, 감당하기 힘든 수준은 아닙니다.", en: "I have a lot on my plate at work, but it’s nothing I can’t handle." },
                { id: 11, kr: "이건 파티해야 돼!", en: "That calls for a party!" },
                { id: 12, kr: "죄송해요. 못 들었어요.", en: "I’m sorry. I didn’t catch that." },
                { id: 13, kr: "어쩐지 기분이 상쾌해 보이더라.", en: "No wonder you look so refreshed." },
                { id: 14, kr: "이 말을 어떻게 꺼내야 할지.", en: "I can’t think of the right thing to say." },
                { id: 15, kr: "새해가 코앞이네. 그나저나 너 부모님 댁에 갈 거야?", en: "New Year’s is just around the corner. Are you going to your parent’s house, by the way?" },
                { id: 16, kr: "시간 가는 줄도 몰랐네.", en: "I wasn’t aware of the time." },
                { id: 17, kr: "이제 그만 들어가 보렴.", en: "I think I will have to let you go." },
                { id: 18, kr: "설이 일주일도 채 안 남았네.", en: "Lunar New Year is less than a week away." },
                { id: 19, kr: "일이 밀려서 점심 먹을 시간도 없어.", en: "I don’t have time to grab lunch. I’m behind on work." },
                { id: 20, kr: "누가 아니래.", en: "That’s for sure." },
                { id: 21, kr: "검은색이 진리지.", en: "You can’t go wrong with black." },
                { id: 22, kr: "나가서 바람이나 좀 쐬고 오자.", en: "Let’s go out for some fresh air." },
                { id: 23, kr: "무슨 일이신가요? 제 검사 결과 관련된 건가요?", en: "What is this about? Is this about my test results?" },
                { id: 24, kr: "그게 훨씬 더 낫겠다.", en: "That sounds like an even better plan to me." },
                { id: 25, kr: "저희가 뭐 진지하게 사귀고 그런 건 아니에요.", en: "It’s not like we are in a serious relationship or anything." },
                { id: 26, kr: "정말로 내 초밥 안 먹어 볼 거야? 진짜 맛있는데.", en: "Are you sure you don’t want to try my sushi? It’s really good." },
                { id: 27, kr: "아까는 미안. 휴대폰 배터리가 나가서.", en: "Sorry about earlier. My phone died." },
                { id: 28, kr: "도착하려면 아직 좀 남았어요.", en: "We are not there yet." },
                { id: 29, kr: "휴대폰 충전기를 회사에 놔두고 왔네.", en: "I forgot my phone charger at work." },
                { id: 30, kr: "이런 경우는 또 처음 보네요.", en: "I have never seen anything like this." },
                { id: 31, kr: "나 저녁 사가려고 한솥도시락에 있어.", en: "I am at Hansot, picking up dinner." },
                { id: 32, kr: "그냥 식당 예약한 것 취소하고 집에 있으면 어떨까 하는데.", en: "I thought maybe we could cancel the reservation and just stay at home." },
                { id: 33, kr: "진통제를 먹었는데도 두통이 가시질 않아.", en: "I took a painkiller, but that didn’t relieve my migraine." },
                { id: 34, kr: "추가로 보험 하나 더 가입할 생각은 없습니다.", en: "I am not really interested in buying another insurance plan." },
                { id: 35, kr: "너 잠깐 시간 되니? 내가 작업 중인 로고를 보여주고 싶어서.", en: "Hey, have you got a minute? I’d like to show you the logo I’ve been working on." },
                { id: 36, kr: "명랑 핫도그라고 들어 봤니?", en: "Are you familiar with Myungrang Hotdogs?" },
                { id: 37, kr: "나도 용리단길 안 가 봤는데.", en: "I haven’t been to Yongnidangil, either." },
                { id: 38, kr: "다른 영업직과는 달리, 저는 기본급이 있습니다.", en: "I get a regular salary, unlike other salespeople." },
                { id: 39, kr: "미국은 병원 입원비가 너무 지나치게 비싸요.", en: "In America, hospital stays are unusually expensive." },
                { id: 40, kr: "제가 차로 집까지 모셔다 드린다니까요. 그렇게 하시죠.", en: "Please let me drive you home. I insist." },
                { id: 41, kr: "여기 거실에 해 두신 것이 마음에 듭니다.", en: "I like what you have going on here in the living room." },
                { id: 42, kr: "천만 원이면 중고차치고는 그나마 저렴한 편이긴 한데, 그래도 저한테는 비싸요.", en: "10 million won is relatively cheap for a used car, but it’s still expensive for me." },
                { id: 43, kr: "하루 종일 책상에 앉아 있어서 허리 통증이 생긴 것 같아.", en: "I think I got this back pain from sitting at a desk all day." },
                { id: 44, kr: "비가 오면 머리가 엉망이 돼요.", en: "My hair gets really messy when it rains." },
                { id: 45, kr: "크리스마스를 맞아 이번 주말에 부산에 가려고 해요.", en: "We’re planning on going to Busan this weekend for Christmas." },
                { id: 46, kr: "죄송한데 찾으시는 제품이 재고가 없습니다.", en: "I’m afraid the item you’re looking for is out of stock." },
                { id: 47, kr: "요즘은 꼭 필요한 경우가 아니면 차 안 가지고 다녀요.", en: "I don’t drive nowadays unless I really have to." },
                { id: 48, kr: "가격은 너무 좋네요. 근데 제가 이미 괜찮은 마사지 의자가 있거든요.", en: "That is a great price. I’m afraid I already have a decent massage chair, though." },
                { id: 49, kr: "민수는 머리를 삼만 삼천 원 주고 자른대. 난 도저히 이해가 안 가.", en: "Minsu said he spends 33,000 won on his haircut. That doesn’t make sense to me." },
                { id: 50, kr: "그 둘은 결국 잘 안 될 거야.", en: "I don’t think things are gonna work out between them." }
            ],
            // --- 2. 구동사 (Set 01~10 기반 50개 선정) ---
            phrasal: [
                { id: 51, kr: "자, 이 숫자들을 더해 보자.", en: "Let’s add up these numbers now." },
                { id: 52, kr: "하루 10분의 연습도 쌓이면 정말 큽니다.", en: "It might not seem like much, but 10 minutes of practice every day really adds up." },
                { id: 53, kr: "뭔가 앞뒤가 안 맞잖아.", en: "Something doesn’t add up." },
                { id: 54, kr: "모자가 강풍에 날아가 버렸다.", en: "My hat blew away in the strong wind." },
                { id: 55, kr: "정말 놀랐어요!", en: "You really blew me away!" },
                { id: 56, kr: "제 차가 고속 도로에서 고장이 났습니다.", en: "My car broke down on the highway." },
                { id: 57, kr: "근무시간 관련 협상이 결렬된 것은 불가피했습니다.", en: "It was inevitable that the negotiation over working hours broke down." },
                { id: 58, kr: "저는 무너졌습니다.", en: "When the doctor said I could never play professionally again, I broke down." },
                { id: 59, kr: "스케줄을 세부적으로 말씀드릴게요.", en: "Let me break down the schedule." },
                { id: 60, kr: "사실 Susie가 헤어지자고 해서 헤어진 거야.", en: "She broke up with me, actually." },
                { id: 61, kr: "자, 얘들아. 3명씩 조를 나누어라.", en: "OK, class. I need you to break up into groups of three." },
                { id: 62, kr: "네 말이 끊겨서 들려.", en: "You are breaking up." },
                { id: 63, kr: "지난 학기에 배운 내용을 다시 한번 복습해 보겠습니다.", en: "I’d like us to brush up on what we learned last semester." },
                { id: 64, kr: "커피나 차 좀 드시겠어요?", en: "Would you care for some coffee or tea?" },
                { id: 65, kr: "저는 무서운 영화를 별로 좋아하지 않아요.", en: "I don’t really care for scary movies." },
                { id: 66, kr: "제가 휴가 가는 동안 우리 집 식물 좀 돌봐 주실 수 있으세요?", en: "Could you care for my plants while I’m on vacation?" },
                { id: 67, kr: "헐렁한 셔츠와 바지가 다시 유행할 줄은 몰랐다.", en: "I didn’t expect baggy shirts and pants to catch on again." },
                { id: 68, kr: "그는 선생님이 그게 뭐냐고 물을 때까지 눈치를 못 챘다.", en: "He didn’t catch on until the teacher asked him about it." },
                { id: 69, kr: "나는 너희들 유머가 전혀 이해가 안 되는데….", en: "I can never catch on to your jokes…" },
                { id: 70, kr: "그동안 밀린 이야기가 정말 많잖아.", en: "We have a lot to catch up on." },
                { id: 71, kr: "이번 주말엔 밀린 잠을 좀 자야겠어.", en: "I’m going to catch up on sleep this weekend." },
                { id: 72, kr: "할머니 안부는 확인해 본 거야?", en: "Have you checked in on Grandma?" },
                { id: 73, kr: "오븐에 닭고기 익히고 있는데, 확인 좀 해 줄 수 있어?", en: "Could you check on the chicken I’m cooking in the oven?" },
                { id: 74, kr: "이 그림 속에 있는 작은 디테일들을 꼼꼼히 봐봐.", en: "Check out the tiny details in this painting." },
                { id: 75, kr: "안에 들어가서 먹을 만한 것 있는지 메뉴 좀 보고 올게.", en: "I am gonna go in and check out their menu to see if they have anything good." },
                { id: 76, kr: "이 시계 자세히 봐봐.", en: "Check out this watch." },
                { id: 77, kr: "불과 몇 분 만에 학생 대부분이 딴생각을 하기 시작했다.", en: "It only took a few minutes before most of the class had mentally checked out." },
                { id: 78, kr: "Samantha는 저랑 헤어지기 전부터 이미 마음이 떠났대요.", en: "Samantha said she had already checked out of the relationship." },
                { id: 79, kr: "걸어서 집에 오다가 자두나무를 봤어요.", en: "I came across a plum tree while walking home." },
                { id: 80, kr: "혹시 파티에 따라가도 될까?", en: "Do you mind if I come along with you to the party?" },
                { id: 81, kr: "공부는 잘되어 가니?", en: "How are your studies coming along?" },
                { id: 82, kr: "당신이 나타날 때까지 나는 방황했어.", en: "I was lost until you came along." },
                { id: 83, kr: "의식이 돌아왔을 때 괜찮았습니다.", en: "I felt okay when I came around." },
                { id: 84, kr: "여름이 될 때마다 다시 태어난 기분이에요.", en: "Every time summer comes around, I feel like a brand-new person." },
                { id: 85, kr: "이번에도 틀림없이 마음이 풀릴 거야.", en: "I’m sure he’ll come around this time, too." },
                { id: 86, kr: "먼 친척들이 올 때면 집이 좀 북적입니다.", en: "Whenever our distant relatives come around, the house feels crowded." },
                { id: 87, kr: "그 병에 붙은 라벨은 물에서 쉽게 벗겨진다.", en: "The labels on the bottles come off so easily in water." },
                { id: 88, kr: "절벽 가장자리로 너무 가까이 가지 마.", en: "I warned my son not to get too close to the cliff’s edge, or else he might fall off." },
                { id: 89, kr: "충돌 사고로 사이드 미러가 떨어져 나갔다.", en: "The right side-view mirror broke off the car in the collision." },
                { id: 90, kr: "그는 실제로는 온화한 사람처럼 보였다.", en: "Despite his rough looks, he actually came off as gentle and caring." },
                { id: 91, kr: "면접에서는 똑똑하다는 인상을 주는 것이 중요하다.", en: "In interviews, it’s important to come across as confident and knowledgeable." },
                { id: 92, kr: "나는 항상 샤워를 할 때 좋은 생각이 떠온다.", en: "I always come up with good ideas in the shower." },
                { id: 93, kr: "이것이 생각나는 최선의 번역입니다.", en: "This is the best translation I could come up with." },
                { id: 94, kr: "내가 이야기하고 있는데 말을 끊으면 정말 짜증난다.", en: "My pet peeve is when people cut me off when I’m in the middle of a story." },
                { id: 95, kr: "나 막혔어(전화/통신).", en: "I’ve just been cut off, is all." },
                { id: 96, kr: "도로에서 차들이 끼어드는 걸 너무 싫어합니다.", en: "My husband hates when people cut him off in the middle of traffic." },
                { id: 97, kr: "최근에 왜 연락도 없고 담을 쌓고 지내?", en: "Why have you been cutting yourself off from us lately?" },
                { id: 98, kr: "마지막 단어가 다 잘렸어.", en: "The last word in every line got cut off." },
                { id: 99, kr: "드디어 루빅큐브 맞추는 방법 알아냈다.", en: "I finally figured out how to solve the Rubik’s Cube." },
                { id: 100, kr: "새 복사기 작동법을 도무지 모르겠어.", en: "I can’t seem to figure out how to work this new photocopier." }
            ],
            // --- 3. 기본동사 (Day 50~100) ---
            basic: [
                { id: 101, kr: "오믈렛 팬은 당신이 가져요.", en: "You can keep the omelet pan." },
                { id: 102, kr: "이 웹사이트에서 계속 에러 메시지가 떠요.", en: "This website keeps giving me an error message." },
                { id: 103, kr: "와, 욕실을 깨끗하게 관리하는 비결이 뭐예요?", en: "Wow, how do you keep your bathroom so clean?" },
                { id: 104, kr: "내일 첫 비행기로 출국합니다.", en: "I’m leaving on the first flight out tomorrow." },
                { id: 105, kr: "배고프면 먹으라고 거기 피자 좀 놔 두었어요.", en: "I left you some pizza over there if you’re hungry." },
                { id: 106, kr: "문을 열어 둘까요?", en: "Do you want me to leave the door open?" },
                { id: 107, kr: "닉을 데려가도 되나요?", en: "Do you mind if I bring Nick with me?" },
                { id: 108, kr: "이번 주말에 부산 내려가요.", en: "I’m heading down to Busan this weekend." },
                { id: 109, kr: "파티에 우리랑 같이 가는 거지요?", en: "Are you coming with us to the party?" },
                { id: 110, kr: "그쪽에서 저를 대기자 명단에 올려 줬어요.", en: "They put me on the waiting list." },
                { id: 111, kr: "악플은 늘 제 기분을 안 좋게 합니다.", en: "Nasty comments always put me in a bad mood." },
                { id: 112, kr: "이 양복 (촉감이) 비싼 느낌이에요.", en: "This suit feels expensive." },
                { id: 113, kr: "당신이 벌써 서른이 되었다니 믿기지가 않는군요.", en: "I can’t believe you’ve already turned 30." },
                { id: 114, kr: "선풍기 좀 저쪽으로 옮겨도 될까요?", en: "Is it OK if I move the fan away from me?" },
                { id: 115, kr: "이 선물 가게는 수녀님들이 운영하고 있어요.", en: "The gift shop is run by nuns." },
                { id: 116, kr: "침은 한 번도 안 맞아 봤어요.", en: "I’ve never tried acupuncture before." },
                { id: 117, kr: "너무 더워서 견디기가 힘들어요.", en: "It’s so hot I can’t stand it." },
                { id: 118, kr: "제가 제일 아끼는 컵 손잡이가 부러졌어요.", en: "The handle on my favorite cup broke." },
                { id: 119, kr: "찬물을 마시면 이가 아파요.", en: "My tooth hurts when I drink cold water." },
                { id: 120, kr: "보니까 점심 때 아무것도 안 먹더군요.", en: "I noticed you never eat anything at lunch." },
                { id: 121, kr: "내일까지 답변 주시기 바랍니다.", en: "I expect an answer by tomorrow." },
                { id: 122, kr: "우리가 지다니 말도 안 돼요.", en: "I just can’t believe we lost." },
                { id: 123, kr: "아침 7시에 버스를 타야 해요.", en: "I have to catch the bus at 7 a.m." },
                { id: 124, kr: "너무 비싸요.", en: "It costs too much." },
                { id: 125, kr: "2만 원이 제가 제시할 수 있는 최대 금액입니다.", en: "Twenty thousand won is the most I can offer." },
                { id: 126, kr: "체중 감량을 위해서는 절제력이 필요합니다.", en: "Losing weight requires discipline." },
                { id: 127, kr: "지난주 워크숍에서 자신감을 많이 얻었어요.", en: "I gained a lot of confidence at the English speaking workshop last week." },
                { id: 128, kr: "오사카에 친구들이 있다고 하지 않았어요?", en: "Didn’t you mention that you have friends in Osaka?" },
                { id: 129, kr: "그 여자 같은 사람은 도저히 감당이 안 돼요.", en: "I can’t handle people like her." },
                { id: 130, kr: "아이스커피는 사계절 잘 팔립니다.", en: "Iced coffee sells year-round." },
                { id: 131, kr: "나비넥타이는 저한테 안 어울려요.", en: "Bow ties don’t suit me." },
                { id: 132, kr: "어제 모임에 못 가서 너무 미안해요.", en: "I’m very sorry for missing our meetup yesterday." },
                { id: 133, kr: "경기가 연장전 끝에 4대 3으로 끝났어요.", en: "The game finished 4-3 in extra innings." },
                { id: 134, kr: "이 케이크 다 못 먹겠어요. 너무 달아요.", en: "I can’t finish this cake—it’s too sweet." },
                { id: 135, kr: "이 머그잔들은 이 수납장에 둬야 해요.", en: "These mugs belong in this cupboard." },
                { id: 136, kr: "이 근처에 집 살 형편이 되면 좋을 텐데요.", en: "I wish I could afford to buy a place around here." },
                { id: 137, kr: "다들 자리 잡고 앉으시면 시작하겠습니다.", en: "Please grab a seat and we’ll get started." },
                { id: 138, kr: "우리가 다시 결혼 생활을 잘할 수 있을지 모르겠어요.", en: "I’m not sure if we can fix our marriage." },
                { id: 139, kr: "그 사람들 당해도 싸다고 생각해요.", en: "I think they got what they deserve." },
                { id: 140, kr: "저는 이 호텔에 묵어도 괜찮아요.", en: "I wouldn’t mind staying at this hotel." },
                { id: 141, kr: "결혼기념일을 맞아 방을 예약했어요.", en: "I booked us a room at The Shilla Seoul for our anniversary." },
                { id: 142, kr: "당신은 항상 뭔가 하느라 바쁜 것 같아요.", en: "You always seem busy with something." },
                { id: 143, kr: "노트북이 느려지면 정말 짜증이 나요.", en: "It really bothers me when my laptop is slow." },
                { id: 144, kr: "그는 문자에 답장하는 법이 없어요.", en: "He never bothers texting back." },
                { id: 145, kr: "잠깐 시간 내서 저 좀 도와주실 수 있나요?", en: "Can you spare a moment to help me with something." },
                { id: 146, kr: "저녁 먹기 전에 사탕 먹지 말아요! 입맛 없어져요!", en: "Don’t eat candy before dinner! You’ll ruin your appetite!" },
                { id: 147, kr: "은행 대출금 다 갚았어요.", en: "I don’t owe the bank anything." },
                { id: 148, kr: "일주일 동안 카페인 끊었어요.", en: "I quit caffeine for a week." },
                { id: 149, kr: "사람을 보내서 데리러 가게 할게요.", en: "I’ll arrange for someone to pick you up." },
                { id: 150, kr: "저는 하체 운동을 절대 거르지 않아요.", en: "I never skip leg day." }
            ]
        };

        const App = () => {
            const [view, setView] = useState('home'); // home, quiz, result
            const [day, setDay] = useState(1);
            const [quizSet, setQuizSet] = useState([]);
            const [currentIndex, setCurrentIndex] = useState(0);
            const [showAnswer, setShowAnswer] = useState(false);
            const [difficultOnes, setDifficultOnes] = useState([]);

            const prepareQuiz = (selectedDay) => {
                setDay(selectedDay);
                const startIdx = (selectedDay - 1) * 5;
                const endIdx = startIdx + 5;

                // 각 카테고리에서 해당 날짜의 5문장씩 추출
                const slice1 = RAW_DATA.conversation.slice(startIdx, endIdx);
                const slice2 = RAW_DATA.phrasal.slice(startIdx, endIdx);
                const slice3 = RAW_DATA.basic.slice(startIdx, endIdx);

                const mixed = [...slice1, ...slice2, ...slice3].sort(() => Math.random() - 0.5);
                setQuizSet(mixed);
                setCurrentIndex(0);
                setShowAnswer(false);
                setDifficultOnes([]);
                setView('quiz');
            };

            const handleNext = (isHard) => {
                if (isHard) {
                    setDifficultOnes(prev => [...prev, quizSet[currentIndex]]);
                }
                if (currentIndex < quizSet.length - 1) {
                    setCurrentIndex(prev => prev + 1);
                    setShowAnswer(false);
                } else {
                    setView('result');
                }
            };

            if (view === 'home') {
                return (
                    <div className="max-w-md mx-auto p-6">
                        <header className="text-center mb-10 mt-10">
                            <h1 className="text-3xl font-black text-slate-800 tracking-tight">SPEED QUIZ</h1>
                            <p className="text-slate-500 mt-2 font-medium">10일간 150문장 마스터</p>
                        </header>
                        <div className="grid grid-cols-2 gap-4">
                            {[...Array(10)].map((_, i) => (
                                <button key={i} onClick={() => prepareQuiz(i + 1)} className="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 hover:border-blue-500 hover:shadow-md transition-all text-left">
                                    <span className="block text-blue-500 font-bold text-xs uppercase mb-1">Day</span>
                                    <span className="text-2xl font-black text-slate-700">{i + 1}</span>
                                </button>
                            ))}
                        </div>
                    </div>
                );
            }

            if (view === 'quiz') {
                const current = quizSet[currentIndex];
                return (
                    <div className="max-w-md mx-auto p-6 h-screen flex flex-col">
                        <div className="flex justify-between items-center mb-6">
                            <span className="px-3 py-1 bg-slate-200 rounded-full text-xs font-bold text-slate-600">Day {day}</span>
                            <span className="text-sm font-bold text-slate-400">{currentIndex + 1} / 15</span>
                        </div>
                        
                        <div 
                            className="flex-1 bg-white rounded-3xl shadow-xl p-8 flex flex-col items-center justify-center text-center border-b-8 border-blue-100 cursor-pointer touch-none"
                            onClick={() => setShowAnswer(!showAnswer)}
                        >
                            <div className="text-slate-400 text-xs font-bold mb-4 uppercase tracking-tighter">아래 문장을 영어로 말해보세요</div>
                            <h2 className="text-2xl font-bold text-slate-800 leading-snug mb-10">{current.kr}</h2>
                            
                            {showAnswer ? (
                                <div className="animate-bounceIn">
                                    <div className="w-8 h-1 bg-blue-500 mx-auto mb-6"></div>
                                    <p className="text-2xl font-black text-blue-600 leading-tight">{current.en}</p>
                                </div>
                            ) : (
                                <div className="text-slate-300 flex flex-col items-center">
                                    <i className="fa-solid fa-hand-pointer mb-2"></i>
                                    <span className="text-xs font-bold uppercase">Tap to reveal answer</span>
                                </div>
                            )}
                        </div>

                        <div className="grid grid-cols-2 gap-4 mt-8 pb-10">
                            <button onClick={() => handleNext(true)} className="py-4 bg-slate-100 text-slate-500 font-bold rounded-2xl hover:bg-red-50 hover:text-red-500 transition-all">잘 안 외워져요 ⭐</button>
                            <button onClick={() => handleNext(false)} className="py-4 bg-blue-600 text-white font-bold rounded-2xl shadow-lg shadow-blue-200 active:scale-95 transition-all">알고 있어요!</button>
                        </div>
                    </div>
                );
            }

            if (view === 'result') {
                return (
                    <div className="max-w-md mx-auto p-6">
                        <div className="text-center mb-8 mt-10">
                            <div className="w-20 h-20 bg-green-100 text-green-500 rounded-full flex items-center justify-center text-3xl mx-auto mb-4">
                                <i className="fa-solid fa-check-double"></i>
                            </div>
                            <h2 className="text-2xl font-black text-slate-800">Day {day} 학습 완료!</h2>
                            <p className="text-slate-500 mt-1 font-medium">오늘도 한 걸음 더 나아갔습니다.</p>
                        </div>

                        <div className="bg-white rounded-3xl p-6 shadow-sm border border-slate-200">
                            <h3 className="font-bold text-slate-800 mb-4 flex items-center">
                                <i className="fa-solid fa-star text-yellow-400 mr-2"></i>
                                복습이 필요한 문장 ({difficultOnes.length})
                            </h3>
                            <div className="space-y-4 max-h-[350px] overflow-y-auto pr-2">
                                {difficultOnes.length > 0 ? difficultOnes.map((item, i) => (
                                    <div key={i} className="p-4 bg-slate-50 rounded-xl border border-slate-100">
                                        <div className="text-blue-600 font-bold mb-1">{item.en}</div>
                                        <div className="text-slate-500 text-sm">{item.kr}</div>
                                    </div>
                                )) : <div className="text-center py-10 text-slate-400 font-medium">모든 문장을 마스터했습니다!</div>}
                            </div>
                        </div>

                        <button onClick={() => setView('home')} className="w-full py-4 bg-slate-800 text-white font-bold rounded-2xl mt-8 shadow-xl">메인으로 돌아가기</button>
                    </div>
                );
            }
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
