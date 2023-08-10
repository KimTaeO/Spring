## 연관관계 매핑
* 테이블을 설계하다 보면 두 테이블 간에 관계를 생성해야 하는 경우가 생긴다. 스프링에서는 JPA라는 ORM을 통해 보통 자바 객체와 테이블을 매핑하는데 객체들끼리도 매핑을 해야 테이블에도 반영된다
* 테이블의 간에 관계를 생성하는 것은 두 테이블 중에 종속테이블의 PK를 주인테이블에 FK로 저장하여 관계를 생성하게 되고, 테이블은 외래키를 통해 JOIN 연산을 할 수 있기 떄문에 양방향이다
* 객체에서 관계를 나타내는것은 참조를 통해 나타내고 한쪽에서 다른쪽을 참조하기 때문에 단방향일 수 밖에 없다, 하지만 객체를 단방향으로 매핑하여도 매핑한 시점에서 테이블 간의 연관관계는 양방향으로 만들어져있따

* 객체 그래프 탐색: 한 객체를 통해 연관된 다른 객체를 조회하는 것을 의미한다

## 연관관계 매핑의 특징
* 방향: 단방향과, 양방향으로 나뉘는데, 두 테이블 간에 관계가 성립해 있을 때, 두 테이블중 한 테이블만이 다른 테이블을 참조하는 것을 단방향, 서로 참조하는 것을 양방향이라 한다
* 다중성: 상황에 따라 1:1, 1:N, N:M 세가지 종류로 나뉘게 된다. 이 종류들은 연관관계가 지어지는 테이블과 객체들의 개수이다
* 연관관계의 주인: 외래키를 가지는 테이블을 연관관계의 주인이라 한다


### 1:1 단방향
* 1:1 관계는 양쪽이 서로 하나의 관계만 가진다
* 테이블 관계에서는 둘중 어느 테이블이든 외래 키를 가질 수 있다
> 객체지향적인 개념으로는 주 테이블에 외래키를 놓는 방식을 선호하고, DB관점에서는 나중에 관계가 바뀌게 될때 수정이 용이한 대상 테이블에 외래 키를 놓는 방식을 선호한다

* 코틀린으로 1:1 단방향 관계 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    @Column(nullable = false)
    val name: String
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @OneToOne
    @JoinColumn(name = "library_id", nullable = false)
    val library: Library
)
```
> 이 관계에서 관계의 주인이 바뀌어도 상관은 없다

### 1:1 양방향
* 1:1 단방향 매핑에서 객체 그래프 탐색 기능이 추가되어 대상 테이블에서 ```read-Only```로 주 테이블을 조회하는 것이 가능해진 것이 1:1 단방향과의 차이점이다
* 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    @Column(nullable = false)
    val name: String

    @OnetoOne(mappedBy = "library")
    val librarian: Librarian
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @OneToOne
    @JoinColumn(name = "library_id", nullable = false)
    val library: Library
)
```
> @OneToOne의 ```mappedBy```속성에 참조를 하고있지 않은 객체를 적어주어 추가한다

### 1:N 단방향
* N 방향이 여러개의 관계를 가질 수 있다
* 테이블에서는 N 방향 테이블이 외래키를 가져야 한다
* 코틀린으로 1:N 단방향 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @OneToMany
    @JoinColumn(name = "library_id")
    val librarians: List<Librarian>
)


@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,
)
```

### 1:N 양방향
* JPA에서 지원하지 않으며, 1:N 특성상 참조하는 객체의 다른 객체와 연결된 테이블에 외래 키가 생성된다
* 1:N 대신 N:1매핑에 ```read-Only```를 직접 설정하여 1:N 양방향을 구현할 수 있다
* 코틀린으로 1:N 양방향 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @OneToMany
    @JoinColumn(name = "library_id")
    val librarians: List<Librarian>
)


@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @ManyToOne
    @JoinColumn(name = "library_id", insertable = false, updatable = false) // Insert, Update 비활성화
    val library: Library
)
```

> 1:N 연관관계 매핑은 객체가 관리하는 외래 키가 다른 테이블에 있다는 특징이 있다. 이는 곧 객체에 값이 추가되거나 변동될 때 다른 테이블에 UPDATE 쿼리를 날려야 하는 성능상 이슈로 다가온다 1:N보다는 N:1을 지향하자

### N:1 단방향
* 1:N과 연관관계의 주인이 객체가 관리하는 외래키랑 같은 테이블에 있다는 점 빼고 거의 동일하다
* 코틀린으로 N:1 단방향 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    val name: String
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @ManyToOne
    @JoinColumn(name = "library_id")
    val library: Library
)
```

### N:1 양방향
* 또한 연관관계의 주인이 객체가 관리하는 외래키랑 같은 테이블에 있어 N:1을 사용하지 않을 이유가 없다
* 코틀린으로 N:1 양방향 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    val name: String,

    @OneToMany(mappedBy = "library")
    val librarians: List<Librarian>
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @ManyToOne
    @JoinColumn(name = "library_id")
    val library: Library
)
```

## N:M
* 관계형 DB는 정규화된 테이블 2개로 N:M 관계를 표현할 수 없다. 따라서 중간에 연결 테이블을 추가해 N:M 관계를 1:N, N:1 관계로 풀어낼 수 있다
* 하지만 객체는 테이블과 다르게 객체 2개로 N:M 관계를 풀어낼 수 있다

### N:M 단방향
* @ManyToMany와 @JoinTable을 통해 연결테이블을 관리하지 않고도 다대다 관계를 풀어낼 수 있다
* 코틀린으로 N:M 양방향 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    val name: String,

    @ManyToMany
    @JoinTable(name = "LIBRARY_LIBRARIAN",
    joinColumns = @JoinColumn(name = "LIBRARIAN_ID"), 
    inverseJoinColumns = @JoinColumn(name = "LIBRARY_ID"))
    val librarians: MutableList<Librarian> = mutableListOf()
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String
)
```
* joinColumns 파라미터는 현재 매핑하려는 엔티티의 JoinColumn 정보로 필요로 한다
* inverseJoinColumns 파라미터는 현재 매핑하려는 엔티티의 반대 엔티티의 JoinColumn 정보를 필요로 한다

### N:M 양방향
* mappedBy를 통해 연관관계의 주인을 정해주는 것 말고는 단방향과 차이가 없다
* 코틀린으로 N:M 양방향 구현 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    val name: String,

    @ManyToMany
    @JoinTable(name = "LIBRARY_LIBRARIAN",
    joinColumns = @JoinColumn(name = "LIBRARIAN_ID"), 
    inverseJoinColumns = @JoinColumn(name = "LIBRARY_ID"))
    val librarians: MutableList<Librarian> = mutablelistof()
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String

    @ManyToMany(mappedBy = "librarians")
    val libraries: MutableList<Library> = mutablelistof()
)
```

### 다대다 연관관계 매핑의 문제점
* @ManyToMany 어노테이션을 사용하면 연결 테이블에 컬럼을 추가하거나 하는 등 테이블을 직접 관리할 수 없게된다
> 따라서 연결 엔티티를 직접 추가해 1:N, N:1 연관관계를 매핑한 다음 N:M 처럼 사용하는 것이다
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    val name: String,

    @OneToMany(mappedBy = "library")
    val librarians: MutableList<Librarian> = mutableListOf<Librarian>()
)

@Entity
class Library_Librarian(
    @Id
    @ManyToOne
    @JoinColumn(name = "LIBRARY_ID")
    val library: Library,
        
    @Id 
    @ManyToOne
    @JoinColumn(name = "LIBRARIAN_ID")
    val librarian: Librarian
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,
    
    @OneToMany(mappedBy = "librarian")
    val libraries: MutableList<Library> = mutableListOf()
)

data class LibraryLibrarianId(
    val library: String,
    val librarian: String
): Serializable
```

* 복합 기본키를 통해 두 테이블의 기본키를 연결 테이블의 기본키로 사용하였다 이를 식별 관계라고 한다
    * 식별자 클래스를 만들어 @IdClass 어노테이션에 지정하였고 식별자 클래스는 Serializable을 구현해야 하며, equals와 hashCode 메소드를 구현해야 하고, 식별자 클래스는 public이어야 한다

> 하지만 이러한 방식은 식별자 클래스를 만들고 equals와 hashCode를 구현하고, @IdClass 등을 사용해야 하는 번거로움을 가지고 있는데 이러한 복잡함을 해결하기 위해서는 새로운 기본 키를 사용하는 방법이 있다

### N:M 새로운 기본 키 사용
* 복합 키를 사용하지 않고 데이터베이스에서 자동으로 생성되는 키를 사용하여 간단히 매핑을 완성할 수 있다 이를 비식별 관계라고 한다
* 코틀린으로 N:M 새로운 기본 키 사용 예시 코드
```Kotlin
@Entity
class Library(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "library_id")
    var id: Long = 0,

    val name: String,

    @OneToMany(mappedBy = "library")
    val librarians: MutableList<Librarian> = mutableListOf<Librarian>()
)

@Entity
class Library_Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    val id: Long = 0L,
    
    @ManyToOne
    @JoinColumn(name = "LIBRARY_ID")
    val library: Library,
    
    @ManyToOne
    @JoinColumn(name = "LIBRARIAN_ID")
    val librarian: Librarian
)

@Entity
class Librarian(
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "librarian_id", nullable = false)
    var id: Long = 0,

    @Column(nullable = false)
    val name: String,

    @OneToMany(mappedBy = "librarian")
    val libraries: MutableList<Library> = mutableListOf()
)
```