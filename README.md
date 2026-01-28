import React, { useState, useEffect, useMemo } from 'react';
import { 
  Star, Play, CheckCircle, ChevronRight, X, 
  RotateCcw, BookOpen, Award, List, AlertCircle 
} from 'lucide-react';

// --- 데이터는 가독성을 위해 상단에 유지 (제공해주신 데이터와 동일) ---
const conversationData = [
  { day: 51, kr: "골프를 더 잘 치고 싶어요.", en: "I want to get better at golf." },
  { day: 52, kr: "바빠서 운동 할 짬이 안 나네요.", en: "I can't seem to find (the) time to exercise." },
  { day: 53, kr: "다이소는 가격을 생각하면 꽤 좋은 물건들을 판다.", en: "Daiso has pretty good products for its low prices" },
  { day: 54, kr: "점심 먹으면서 그동안 못했던 이야기하자.", en: "Let's catch up over lunch." },
  { day: 55, kr: "출근 전에 잠깐 우리 집 들러서 커피 한잔하고 가.", en: "Swing by my place for coffee before work." },
  { day: 56, kr: "죄송한데 선약이 있습니다.", en: "I'm afraid I already have plans." },
  { day: 57, kr: "요새 돈이 좀 궁해.", en: "Money is a bit tight right now." },
  { day: 58, kr: "근무 시간은 어때?", en: "What are the hours like?" },
  { day: 59, kr: "회사에 일이 너무 많긴 한데, 그래도 감당하기 힘든 수준은 아닙니다.", en: "I have a lot on my plate at work, but it's nothing I can't handle." },
  { day: 60, kr: "이건 파티해야 돼!", en: "That calls for a party!" },
  { day: 61, kr: "죄송해요. 못 들었어요.", en: "I'm sorry. I didn't catch that." },
  { day: 62, kr: "어쩐지 기분이 상쾌해 보이더라.", en: "No wonder you look so refreshed." },
  { day: 63, kr: "이 말을 어떻게 꺼내야 할지.", en: "I can't think of the right thing to say." },
  { day: 64, kr: "새해가 코앞이네. 그나저나 너 부모님 댁에 갈 거야?", en: "New Year's is just around the corner. Are you going to your parent's house, by the way?" },
  { day: 65, kr: "시간 가는 줄도 몰랐네.", en: "I wasn't aware of the time." },
  { day: 66, kr: "이제 그만 들어가 보렴.", en: "I think I will have to let you go." },
  { day: 67, kr: "설이 일주일도 채 안 남았네.", en: "Lunar New Year is less than a week away." },
  { day: 68, kr: "일이 밀려서 점심 먹을 시간도 없어.", en: "I don't have time to grab lunch. I'm behind on work." },
  { day: 69, kr: "누가 아니래.", en: "That's for sure." },
  { day: 70, kr: "검은색이 진리지.", en: "You can't go wrong with black." },
  { day: 71, kr: "나가서 바람이나 좀 쐬고 오자.", en: "Let's go out for some fresh air." },
  { day: 72, kr: "무슨 일이신가요? 제 검사 결과 관련된 건가요?", en: "What is this about? Is this about my test results?" },
  { day: 73, kr: "그게 훨씬 더 낫겠다.", en: "That sounds like an even better plan to me." },
  { day: 74, kr: "저희가 뭐 진지하게 사귀고 그런 건 아니에요.", en: "It's not like we are in a serious relationship or anything." },
  { day: 75, kr: "정말로 내 초밥 안 먹어 볼 거야? 진짜 맛있는데.", en: "Are you sure you don't want to try my sushi? It's really good." },
  { day: 76, kr: "아까는 미안. 휴대폰 배터리가 나가서.", en: "Sorry about earlier. My phone died." },
  { day: 77, kr: "도착하려면 아직 좀 남았어요.", en: "We are not there yet." },
  { day: 78, kr: "휴대폰 충전기를 회사에 놔두고 왔네.", en: "I forgot my phone charger at work." },
  { day: 79, kr: "이런 경우는 또 처음 보네요.", en: "I have never seen anything like this." },
  { day: 80, kr: "나 저녁 사가려고 한솥도시락에 있어.", en: "I am at Hansot, picking up dinner." },
  { day: 81, kr: "그냥 식당 예약한 것 취소하고 집에 있으면 어떨까 하는데.", en: "I thought maybe we could cancel the reservation and just stay at home." },
  { day: 82, kr: "진통제를 먹었는데도 두통이 가시질 않아.", en: "I took a painkiller, but that didn't relieve my migraine." },
  { day: 83, kr: "추가로 보험 하나 더 가입할 생각은 없습니다.", en: "I am not really interested in buying another insurance plan." },
  { day: 84, kr: "너 잠깐 시간 되니? 내가 작업 중인 로고를 보여주고 싶어서.", en: "Hey, have you got a minute? I'd like to show you the logo I've been working on." },
  { day: 85, kr: "명랑 핫도그라고 들어 봤니?", en: "Are you familiar with Myungrang Hotdogs?" },
  { day: 86, kr: "나도 용리단길 안 가 봤는데.", en: "I haven't been to Yongnidangil, either." },
  { day: 87, kr: "다른 영업직과는 달리, 저는 기본급이 있습니다.", en: "I get a regular salary, unlike other salespeople." },
  { day: 88, kr: "미국은 병원 입원비가 너무 지나치게 비싸요.", en: "In America, hospital stays are unusually expensive." },
  { day: 89, kr: "제가 차로 집까지 모셔다 드린다니까요. 그렇게 하시죠.", en: "Please let me drive you home. I insist." },
  { day: 90, kr: "여기 거실에 해 두신 것이 마음에 듭니다.", en: "I like what you have going on here in the living room." },
  { day: 91, kr: "천만 원이면 중고차치고는 그나마 저렴한 편이긴 한데, 그래도 저한테는 비싸요.", en: "10 million won is relatively cheap for a used car, but it's still expensive for me." },
  { day: 92, kr: "하루 종일 책상에 앉아 있어서 허리 통증이 생긴 것 같아.", en: "I think I got this back pain from sitting at a desk all day." },
  { day: 93, kr: "비가 오면 머리가 엉망이 돼요.", en: "My hair gets really messy when it rains." },
  { day: 94, kr: "크리스마스를 맞아 이번 주말에 부산에 가려고 해요.", en: "We're planning on going to Busan this weekend for Christmas." },
  { day: 95, kr: "죄송한데 찾으시는 제품이 재고가 없습니다.", en: "I'm afraid the item you're looking for is out of stock." },
  { day: 96, kr: "요즘은 꼭 필요한 경우가 아니면 차 안 가지고 다녀요.", en: "I don't drive nowadays unless I really have to." },
  { day: 97, kr: "가격은 너무 좋네요. 근데 제가 이미 괜찮은 마사지 의자가 있거든요.", en: "That is a great price. I'm afraid I already have a decent massage chair, though." },
  { day: 98, kr: "민수는 머리를 삼만 삼천 원 주고 자른대. 난 도저히 이해가 안 가.", en: "Minsu said he spends 33,000 won on his haircut. That doesn't make sense to me." },
  { day: 99, kr: "그 둘은 결국 잘 안 될 거야.", en: "I don't think things are gonna work out between them." },
  { day: 100, kr: "내가 제대로 하고 있는 건지 잘 모르겠어.", en: "I'm not quite sure if I am doing it right." },
];

const phrasalData = [
  { day: 51, kr: "우리 중 일부를 내보내야 할까 봐 걱정이야.", en: "I'm worried that they might have to let some of us go." },
  { day: 52, kr: "우리 새 차 사는 거 고려해 봐야 할 것 같아.", en: "Maybe we should look at buying a new car." },
  { day: 53, kr: "그녀는 대학을 안 나온 사람을 얕잡아 본다.", en: "She looks down on people who haven't gone to college." },
  { day: 54, kr: "제가 한번 살펴보고 Kyle 엄마에게 전화해 보겠습니다.", en: "I'll look into it and give his mom a call." },
  { day: 55, kr: "내가 과제 한 거 훑어봐 주고 잘못된 거 있는지 확인 좀 해 줘.", en: "I need you to look over my assignment for me and check if anything is wrong." },
  { day: 56, kr: "한번 찾아보자.", en: "Let's look it up." },
  { day: 57, kr: "승진한 이후로 일이 잘 풀리고 있습니다.", en: "Things have been looking up ever since I got promoted." },
  { day: 58, kr: "먼저 양쪽 끝을 좀 느슨하게 해야 해.", en: "you need to loosen up both ends first." },
  { day: 59, kr: "아쉽지만 이 방에 만족하자.", en: "Let's just make do with this." },
  { day: 60, kr: "소음 때문에 무슨 내용인지 알아들을 수가 없었다.", en: "I couldn't make out what it said over the noise." },
  { day: 61, kr: "허비한 시간을 만회할 수는 없다.", en: "You can't make up for lost time." },
  { day: 62, kr: "휴대폰 그만 만지작거려.", en: "Stop messing around with your phone." },
  { day: 63, kr: "공연 때 대사 몇 개를 잘못 말했지 뭐야.", en: "During the play, I messed up several of my lines." },
  { day: 64, kr: "늦은 시간에 커피 마시지 말아야겠어. 수면을 방해해.", en: "it messes with my sleep." },
  { day: 65, kr: "이건 너무 중요한 기회야! 놓칠 수 없어.", en: "This is a huge chance! I'm not going to miss out on it." },
  { day: 66, kr: "저녁 먹고, 조용한 와인바로 이동해서 이야기를 나누었어.", en: "We had dinner and moved on to a quiet wine bar where we talked for hours." },
  { day: 67, kr: "답을 모르겠으면, 다음 문제로 넘어가.", en: "move on and try again when you've finished the easier ones." },
  { day: 68, kr: "재미 교포라고 해도 믿을 것이다.", en: "he can pass for a Korean-American." },
  { day: 69, kr: "그는 정상적인 은행원인 척 거짓 삶을 살았다.", en: "He was living his life, passing for a normal banker and husband." },
  { day: 70, kr: "세일하는 걸 그냥 지나칠 수가 없었다.", en: "I couldn't pass up (on) the sale." },
  { day: 71, kr: "내가 회사로 태우러 갈게.", en: "I will pick you up from work." },
  { day: 72, kr: "집에 가는 길에 저녁거리 좀 사 갈게.", en: "I'll pick up something for dinner on my way home." },
  { day: 73, kr: "어제 마친 부분부터 다시 이야기할까요?", en: "OK, guys, let's pick up where we left off yesterday." },
  { day: 74, kr: "민지는 생각보다 빠르게 언어의 뉘앙스를 알아차리더군요.", en: "Minji picked up on the nuances of the language faster than expected." },
  { day: 75, kr: "나는 소화하기 어렵다.", en: "I can't pull it off." },
  { day: 76, kr: "할 수 있겠어? (성공할 수 있겠어?)", en: "Do you think you can pull it off?" },
  { day: 77, kr: "반드시 운동기구를 제자리에 두기 바랍니다.", en: "Please be sure to put away weights after you use them." },
  { day: 78, kr: "저축할 돈이 거의 남지 않아요.", en: "I don't have any money left to put away after paying all my bills." },
  { day: 79, kr: "휴대폰 내려놓고 저녁 먹어야지!", en: "Put down your phone and eat your dinner!" },
  { day: 80, kr: "제 생각을 적으려고 합니다.", en: "I try to put down my thoughts on paper when I have too many things to do." },
  { day: 81, kr: "꼭 그렇게 나를 깎아내려야 했어?", en: "Why did you have to put me down in front of everybody like that?" },
  { day: 82, kr: "담배 냄새가 너무 싫어서 흡연자와는 절대 사귀지 않아요.", en: "I would never date a smoker because the smell of tobacco really puts me off." },
  { day: 83, kr: "결정하기 전에 입어 보려고요.", en: "I want to try this on before I decide." },
  { day: 84, kr: "직접 조립할 수 없어요.", en: "I can't put together anything that requires a drill." },
  { day: 85, kr: "우리 머리를 맞대어서 재미있는 여행 계획을 짜 보자.", en: "Let's put our heads together and plan a fun trip this summer." },
  { day: 86, kr: "인스타에 올리자.", en: "Let's put up these pictures of the decor on Instagram before we head out." },
  { day: 87, kr: "Jeff가 몇 주간 재워 줬답니다.", en: "Jeff put me up for a few weeks." },
  { day: 88, kr: "네 아내가 너를 어떻게 참지?", en: "How does your wife put up with you?" },
  { day: 89, kr: "여기서 보다니 반갑다! (우연히 마주침)", en: "Nice running into you!" },
  { day: 90, kr: "착륙 직전에 난기류를 만났습니다.", en: "We ran into some turbulence just before landing." },
  { day: 91, kr: "여자애들에게 잘 보이려고 과시하는 거지.", en: "he's trying to show off to impress some girls." },
  { day: 92, kr: "게으름 피우지 않으려고 노력 중입니다.", en: "I've been trying to stop slacking off." },
  { day: 93, kr: "다 잘 해결된 거야?", en: "Did you get everything sorted out?" },
  { day: 94, kr: "좀 더 앉아 있다가 가자.", en: "Let's stick around for a while." },
  { day: 95, kr: "끝까지 포기하지 않으면 정규직이 될 거야.", en: "all you need to do is stick it out until the end and you'll be a full-time employee." },
  { day: 96, kr: "은행에 들러 현금 좀 찾아 올 수 있을까?", en: "could you stop by the bank and get some cash?" },
  { day: 97, kr: "내가 한 말 취소할게.", en: "I take that back." },
  { day: 98, kr: "너는 안경 벗으면 완전 딴 사람 같아 보여.", en: "You look like a different person when you take off your glasses." },
  { day: 99, kr: "내가 일을 너무 많이 떠맡았어.", en: "I have taken on too much work." },
  { day: 100, kr: "나한테 화풀이하지 마!", en: "Don't take your anger out on me!" },
];

const basicVerbData = [
  { day: 51, kr: "이 웹사이트에서 계속 에러 메시지가 떠요.", en: "This website keeps giving me an error message." },
  { day: 52, kr: "와, 욕실을 깨끗하게 관리하는 비결이 뭐예요?", en: "Wow, how do you keep your bathroom so clean?" },
  { day: 53, kr: "내일 첫 비행기로 출국합니다.", en: "I’m leaving on the first flight out tomorrow." },
  { day: 54, kr: "배고프면 먹으라고 거기 피자 좀 놔 두었어요.", en: "I left you some pizza over there if you’re hungry." },
  { day: 55, kr: "문을 열어 둘까요?", en: "Do you want me to leave the door open?" },
  { day: 56, kr: "닉을 데려가도 되나요?", en: "Do you mind if I bring Nick with me?" },
  { day: 57, kr: "이번 주말에 부산 내려가요.", en: "I’m heading down to Busan this weekend." },
  { day: 58, kr: "파티에 우리랑 같이 가는 거지요?", en: "Are you coming with us to the party?" },
  { day: 59, kr: "그쪽에서 저를 대기자 명단에 올려 줬어요.", en: "They put me on the waiting list." },
  { day: 60, kr: "악플은 늘 제 기분을 안 좋게 합니다.", en: "Nasty comments always put me in a bad mood." },
  { day: 61, kr: "이 양복 (촉감이) 비싼 느낌이에요.", en: "This suit feels expensive." },
  { day: 62, kr: "당신이 벌써 서른이 되었다니 믿기지가 않는군요.", en: "I can’t believe you’ve already turned 30." },
  { day: 63, kr: "선풍기 좀 저쪽으로 옮겨도 될까요?", en: "Is it OK if I move the fan away from me?" },
  { day: 64, kr: "이 선물 가게는 수녀님들이 운영하고 있어요.", en: "The gift shop is run by nuns." },
  { day: 65, kr: "침은 한 번도 안 맞아 봤어요.", en: "I’ve never tried acupuncture before." },
  { day: 66, kr: "너무 더워서 견디기가 힘들어요.", en: "It’s so hot I can’t stand it." },
  { day: 67, kr: "제가 제일 아끼는 컵 손잡이가 부러졌어요.", en: "The handle on my favorite cup broke." },
  { day: 68, kr: "찬물을 마시면 이가 아파요.", en: "My tooth hurts when I drink cold water." },
  { day: 69, kr: "보니까 점심 때 아무것도 안 먹더군요.", en: "I noticed you never eat anything at lunch." },
  { day: 70, kr: "내일까지 답변 주시기 바랍니다.", en: "I expect an answer by tomorrow." },
  { day: 71, kr: "우리가 지다니 말도 안 돼요.", en: "I just can’t believe we lost." },
  { day: 72, kr: "아침 7시에 버스를 타야 해요.", en: "I have to catch the bus at 7 a.m." },
  { day: 73, kr: "너무 비싸요.", en: "It costs too much." },
  { day: 74, kr: "2만 원이 제가 제시할 수 있는 최대 금액입니다.", en: "Twenty thousand won is the most I can offer." },
  { day: 75, kr: "체중 감량을 위해서는 절제력이 필요합니다.", en: "Losing weight requires discipline." },
  { day: 76, kr: "지난주 영어 말하기 워크숍에서 자신감을 많이 얻었어요.", en: "I gained a lot of confidence at the English speaking workshop last week." },
  { day: 77, kr: "오사카에 친구들이 있다고 하지 않았어요?", en: "Didn’t you mention that you have friends in Osaka?" },
  { day: 78, kr: "그 여자 같은 사람은 도저히 감당이 안 돼요.", en: "I can’t handle people like her." },
  { day: 79, kr: "아이스커피는 사계절 잘 팔립니다.", en: "Iced coffee sells year-round." },
  { day: 80, kr: "나비넥타이는 저한테 안 어울려요.", en: "Bow ties don’t suit me." },
  { day: 81, kr: "어제 모임에 못 가서 너무 미안해요.", en: "I’m very sorry for missing our meetup yesterday." },
  { day: 82, kr: "경기가 연장전 끝에 4대 3으로 끝났어요.", en: "The game finished 4-3 in extra innings." },
  { day: 83, kr: "이 케이크 다 못 먹겠어요. 너무 달아요.", en: "I can’t finish this cake—it’s too sweet." },
  { day: 84, kr: "이 머그잔들은 이 수납장에 둬야 해요.", en: "These mugs belong in this cupboard." },
  { day: 85, kr: "이 근처에 집 살 형편이 되면 좋을 텐데요.", en: "I wish I could afford to buy a place around here." },
  { day: 86, kr: "다들 자리 잡고 앉으시면 시작하겠습니다.", en: "Please grab a seat and we’ll get started." },
  { day: 87, kr: "우리가 다시 결혼 생활을 잘할 수 있을지 모르겠어요.", en: "I’m not sure if we can fix our marriage." },
  { day: 88, kr: "그 사람들 당해도 싸다고 생각해요.", en: "I think they got what they deserve." },
  { day: 89, kr: "저는 이 호텔에 묵어도 괜찮아요.", en: "I wouldn’t mind staying at this hotel." },
  { day: 90, kr: "우리 결혼기념일을 맞아서 서울신라호텔에 방을 예약했어요.", en: "I booked us a room at The Shilla Seoul for our anniversary." },
  { day: 91, kr: "당신은 항상 뭔가 하느라 바쁜 것 같아요.", en: "You always seem busy with something." },
  { day: 92, kr: "노트북이 느려지면 정말 짜증이 나요.", en: "It really bothers me when my laptop is slow." },
  { day: 93, kr: "그는 문자에 답장하는 법이 없어요.", en: "He never bothers texting back." },
  { day: 94, kr: "잠깐 시간 내서 저 좀 도와주실 수 있나요?", en: "Can you spare a moment to help me with something?" },
  { day: 95, kr: "저녁 먹기 전에 사탕 먹지 말아요! 입맛 없어져요!", en: "Don’t eat candy before dinner! You’ll ruin your appetite!" },
  { day: 96, kr: "은행 대출금 다 갚았어요.", en: "I don’t owe the bank anything." },
  { day: 97, kr: "일주일 동안 카페인 끊었어요.", en: "I quit caffeine for a week." },
  { day: 98, kr: "사람을 보내서 데리러 가게 할게요.", en: "I’ll arrange for someone to pick you up." },
  { day: 99, kr: "저는 하체 운동을 절대 거르지 않아요.", en: "I never skip leg day." },
  { day: 100, kr: "영화 줄거리 말하지 말아요! 아직 안 봤단 말이에요.", en: "Don’t spoil the movie! I haven’t seen it yet." },
];

const App = () => {
  const [activeDay, setActiveDay] = useState(1);
  const [mode, setMode] = useState('study'); // 'study' | 'quiz' | 'result'
  const [favorites, setFavorites] = useState({});
  const [quizState, setQuizState] = useState({
    currentIdx: 0,
    showAnswer: false,
    score: 0,
    items: [],
    difficultItems: [],
  });

  // Curriculum generation
  const curriculum = useMemo(() => {
    const days = [];
    for (let i = 0; i < 10; i++) {
      const startIndex = i * 5;
      const endIndex = startIndex + 5;
      
      const dayData = [
        ...conversationData.slice(startIndex, endIndex).map((item, idx) => ({ ...item, type: '회화', id: `conv-${item.day}-${idx}` })),
        ...phrasalData.slice(startIndex, endIndex).map((item, idx) => ({ ...item, type: '구동사', id: `phra-${item.day}-${idx}` })),
        ...basicVerbData.slice(startIndex, endIndex).map((item, idx) => ({ ...item, type: '기본동사', id: `basi-${item.day}-${idx}` }))
      ];
      days.push(dayData);
    }
    return days;
  }, []);

  const toggleFavorite = (id) => {
    setFavorites(prev => ({
      ...prev,
      [id]: !prev[id]
    }));
  };

  const startQuiz = () => {
    const shuffled = [...curriculum[activeDay - 1]].sort(() => Math.random() - 0.5);
    setQuizState({
      currentIdx: 0,
      showAnswer: false,
      score: 0,
      items: shuffled,
      difficultItems: [],
    });
    setMode('quiz');
  };

  const handleQuizAction = (gotIt) => {
    const currentItem = quizState.items[quizState.currentIdx];
    const nextIdx = quizState.currentIdx + 1;
    const newScore = gotIt ? quizState.score + 1 : quizState.score;
    
    const newDifficultItems = gotIt 
      ? quizState.difficultItems 
      : [...quizState.difficultItems, currentItem];

    if (nextIdx < quizState.items.length) {
      setQuizState(prev => ({
        ...prev,
        currentIdx: nextIdx,
        showAnswer: false,
        score: newScore,
        difficultItems: newDifficultItems
      }));
    } else {
      setQuizState(prev => ({ 
        ...prev, 
        score: newScore,
        difficultItems: newDifficultItems 
      }));
      setMode('result');
    }
  };

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans p-4 md:p-8">
      {/* Header */}
      <header className="max-w-3xl mx-auto mb-8 text-center">
        <h1 className="text-3xl font-bold text-indigo-700 mb-2 flex items-center justify-center gap-2">
          <Award className="text-amber-500" /> English Sprint Quiz
        </h1>
        <p className="text-slate-500">10일 완성: 회화/구동사/기본동사 150문장 마스터</p>
      </header>

      {/* Day Selector */}
      {mode === 'study' && (
        <div className="max-w-4xl mx-auto mb-6">
          <div className="flex overflow-x-auto pb-2 gap-2 no-scrollbar">
            {[...Array(10)].map((_, i) => (
              <button
                key={i}
                onClick={() => setActiveDay(i + 1)}
                className={`flex-shrink-0 px-5 py-2 rounded-full font-semibold transition-all shadow-sm ${
                  activeDay === i + 1 
                    ? 'bg-indigo-600 text-white shadow-indigo-200' 
                    : 'bg-white text-slate-600 hover:bg-slate-100'
                }`}
              >
                Day {String(i + 1).padStart(2, '0')}
              </button>
            ))}
          </div>
        </div>
      )}

      {/* Main Content */}
      <main className="max-w-2xl mx-auto">
        {mode === 'study' && (
          <div className="space-y-6">
            <div className="flex justify-between items-center bg-white p-4 rounded-2xl shadow-sm border border-slate-100">
              <div>
                <span className="text-sm font-bold text-indigo-500 uppercase tracking-wider">Day {activeDay} 학습 리스트</span>
                <h2 className="text-xl font-bold">오늘의 15문장</h2>
              </div>
              <button 
                onClick={startQuiz}
                className="flex items-center gap-2 bg-indigo-600 text-white px-6 py-3 rounded-xl font-bold hover:bg-indigo-700 transition-transform active:scale-95 shadow-lg shadow-indigo-100"
              >
                <Play size={18} fill="currentColor" /> 퀴즈 시작
              </button>
            </div>

            <div className="space-y-3">
              {curriculum[activeDay - 1].map((item) => (
                <div key={item.id} className="bg-white p-4 rounded-xl shadow-sm border border-slate-100 flex items-start gap-4 group">
                  <div className="mt-1 flex-shrink-0 w-16 text-[10px] font-bold py-1 bg-slate-100 text-slate-500 rounded text-center uppercase tracking-tighter">
                    {item.type}
                  </div>
                  <div className="flex-grow">
                    <p className="font-medium text-slate-800 mb-1">{item.kr}</p>
                    <p className="text-indigo-600 font-bold opacity-0 group-hover:opacity-100 transition-opacity">
                      {item.en}
                    </p>
                  </div>
                  <button 
                    onClick={() => toggleFavorite(item.id)}
                    className={`flex-shrink-0 transition-colors ${favorites[item.id] ? 'text-amber-400' : 'text-slate-300 hover:text-slate-400'}`}
                  >
                    <Star size={20} fill={favorites[item.id] ? "currentColor" : "none"} />
                  </button>
                </div>
              ))}
            </div>
          </div>
        )}

        {mode === 'quiz' && quizState.items.length > 0 && (
          <div className="bg-white rounded-3xl p-8 shadow-xl shadow-slate-200 border border-slate-100 text-center relative overflow-hidden">
            {/* Progress Bar */}
            <div className="absolute top-0 left-0 h-1.5 bg-slate-100 w-full">
              <div 
                className="h-full bg-indigo-500 transition-all duration-300" 
                style={{ width: `${((quizState.currentIdx + 1) / quizState.items.length) * 100}%` }}
              />
            </div>

            <div className="flex justify-between items-center mb-10">
              <span className="text-sm font-bold text-slate-400">
                QUIZ {quizState.currentIdx + 1} / {quizState.items.length}
              </span>
              <button onClick={() => setMode('study')} className="text-slate-400 hover:text-slate-600">
                <X size={24} />
              </button>
            </div>

            <div className="min-h-[160px] flex flex-col justify-center items-center px-4">
              <div className="inline-block px-3 py-1 bg-indigo-50 text-indigo-600 text-xs font-bold rounded-full mb-4">
                {quizState.items[quizState.currentIdx].type}
              </div>
              <h3 className="text-2xl font-bold text-slate-800 leading-tight">
                {quizState.items[quizState.currentIdx].kr}
              </h3>
              
              <div className={`mt-8 transition-all duration-300 ${quizState.showAnswer ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4 pointer-events-none'}`}>
                <p className="text-xl font-bold text-indigo-600 bg-indigo-50 p-6 rounded-2xl border border-indigo-100 shadow-inner">
                  {quizState.items[quizState.currentIdx].en}
                </p>
              </div>
            </div>

            <div className="mt-12 flex flex-col gap-3">
              {!quizState.showAnswer ? (
                <button 
                  onClick={() => setQuizState(prev => ({ ...prev, showAnswer: true }))}
                  className="w-full bg-slate-900 text-white py-4 rounded-2xl font-bold hover:bg-slate-800 transition-all shadow-lg"
                >
                  정답 확인하기
                </button>
              ) : (
                <div className="grid grid-cols-2 gap-4">
                  <button 
                    onClick={() => handleQuizAction(false)}
                    className="bg-slate-100 text-slate-600 py-4 rounded-2xl font-bold hover:bg-slate-200"
                  >
                    더 공부해야해요
                  </button>
                  <button 
                    onClick={() => handleQuizAction(true)}
                    className="bg-green-500 text-white py-4 rounded-2xl font-bold hover:bg-green-600 shadow-lg shadow-green-100"
                  >
                    외웠어요!
                  </button>
                </div>
              )}
            </div>
          </div>
        )}

        {mode === 'result' && (
          <div className="space-y-6">
            <div className="bg-white rounded-3xl p-10 shadow-xl border border-slate-100 text-center">
              <div className="w-20 h-20 bg-amber-100 text-amber-500 rounded-full flex items-center justify-center mx-auto mb-4">
                <Award size={40} />
              </div>
              <h2 className="text-3xl font-bold mb-2">학습 완료!</h2>
              <p className="text-slate-500 mb-8 font-medium">
                Day {activeDay}의 15문장 중 <span className="text-indigo-600 font-bold">{quizState.score}</span>문장을 마스터했습니다.
              </p>
              
              <div className="grid grid-cols-2 gap-4">
                <button 
                  onClick={startQuiz}
                  className="flex items-center justify-center gap-2 bg-slate-100 text-slate-700 py-4 rounded-2xl font-bold hover:bg-slate-200"
                >
                  <RotateCcw size={18} /> 다시 풀기
                </button>
                <button 
                  onClick={() => setMode('study')}
                  className="flex items-center justify-center gap-2 bg-indigo-600 text-white py-4 rounded-2xl font-bold hover:bg-indigo-700 shadow-lg"
                >
                  <List size={18} /> 목록으로
                </button>
              </div>
            </div>

            {quizState.difficultItems.length > 0 && (
              <div className="bg-white rounded-3xl p-6 shadow-xl border border-slate-100">
                <div className="flex items-center gap-2 mb-6 pb-4 border-b border-slate-50">
                  <AlertCircle className="text-rose-500" size={20} />
                  <h3 className="text-lg font-bold text-slate-800">복습이 필요한 문장 ({quizState.difficultItems.length})</h3>
                </div>
                <div className="space-y-4">
                  {quizState.difficultItems.map((item) => (
                    <div key={`result-${item.id}`} className="p-4 rounded-2xl bg-slate-50 border border-slate-100 flex items-start gap-4">
                      <div className="flex-grow">
                        <div className="flex items-center gap-2 mb-1">
                          <span className="text-[10px] font-bold px-2 py-0.5 bg-indigo-100 text-indigo-600 rounded-full uppercase">
                            {item.type}
                          </span>
                        </div>
                        <p className="font-semibold text-slate-800 text-sm mb-1">{item.kr}</p>
                        <p className="text-indigo-600 font-bold text-sm">{item.en}</p>
                      </div>
                      <button 
                        onClick={() => toggleFavorite(item.id)}
                        className={`flex-shrink-0 transition-colors ${favorites[item.id] ? 'text-amber-400' : 'text-slate-300'}`}
                      >
                        <Star size={18} fill={favorites[item.id] ? "currentColor" : "none"} />
                      </button>
                    </div>
                  ))}
                </div>
              </div>
            )}
          </div>
        )}
      </main>

      {/* Footer */}
      <footer className="max-w-2xl mx-auto mt-12 pt-8 border-t border-slate-200 text-center text-slate-400 text-sm">
        <p>© 2026 English Sprint 10-Day Challenge</p>
        <div className="flex justify-center gap-4 mt-2 font-semibold">
          <span className="flex items-center gap-1"><BookOpen size={14}/> 150 Sentences</span>
          <span className="flex items-center gap-1"><Star size={14}/> Favorite Mode</span>
        </div>
      </footer>
      
      {/* Global CSS for hidescroll */}
      <style>{`
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
      `}</style>
    </div>
  );
};

export default App;
