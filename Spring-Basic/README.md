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
