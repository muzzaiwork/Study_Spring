# 스프링을 사용하지 않고, 순수 자바로 개발했을 경우 코드 예
```
public class MemberServiceImpl implements MemberService{

    // DIP 위반
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```
- DIP와 OCP가 지켜지지 않고 있다.
- `private final MemberRepository memberRepository = new MemoryMemberRepository();`
  - MebmerServiceImpl은 **추상화(MemberRepository)에만 의존하는 것이 아니라 구현체(MemoryMemberRepository)에도 의존**하고 있는중이다.

# AppConfig에서 객체 생성, 생성자 주입
- AppConfig.java
  - 객체의 생성과 연결을 담당한다.
```
public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService() {
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
```
- MemberService.java
```
public class MemberServiceImpl implements MemberService{

    // 추상화에만 의존한다.
    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```


![image](https://github.com/muzzaiwork/Study_Spring/assets/31703020/8317fdd3-ef92-4313-b2d3-5dc9699ff3a9)

![image](https://github.com/muzzaiwork/Study_Spring/assets/31703020/bf3e4546-e8f7-4bbf-bb46-4b5f91bde3f4)


