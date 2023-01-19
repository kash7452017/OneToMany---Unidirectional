## OneToMany---Unidirectional(單向)
**創建Review實體類別，包含基本的constructors、fields、getter/setters就不多做說明了**
```
@Entity
@Table(name="review")
public class Review {

	// define fields
	
	// define constructors
	
	// define getter/setters
	
	// define ToString
	
	// annotate fields
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="comment")
	private String comment;
	
	public Review() {
		
	}

	public Review(String comment) {
		this.comment = comment;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getComment() {
		return comment;
	}

	public void setComment(String comment) {
		this.comment = comment;
	}

	@Override
	public String toString() {
		return "Review [id=" + id + ", comment=" + comment + "]";
	}
}
```
**Course實體類別添加對於Review資料表的映射以及FetchType運作方式**
>參考網址：https://docs.oracle.com/javaee/7/api/javax/persistence/JoinColumn.html#name
>
>* 如果聯接是針對使用外鍵映射策略的OneToOne或ManyToOne映射，則外鍵列在源實體的表中或可嵌入。
>* 如果連接是針對使用外鍵映射策略的單向 OneToMany 映射，則外鍵位於目標實體的表中。
>* 如果連接是針對 ManyToMany 映射或使用連接表的 OneToOne 或雙向 ManyToOne/OneToMany映射，則外鍵位於連接表中。
>* 如果連接用於元素集合，則外鍵位於集合表中。
>
>>對於外鍵該存在於哪個位置，我的理解方式是這樣，以Course與Review這個OneToMany(單向)例子來說明，Course代表一的一方，而Review代表多方，在資料表中，每一資料行中的每一欄位都只僅有一組數據，外鍵所代表的是所對應的目標ID，因此以這樣的邏輯來思考，若外鍵存在於Course，那麼一組課程資料需要紀錄每一相關Review資料的ID，這是不合理的；那麼相反來說，當外鍵位於Review表單中，每一評論資料都會各自存在一筆對應的目標CourseID，如此是否就合理了，因此外鍵才會存在於Review表單中
```
@Entity
@Table(name="course")
public class Course {
	
	// define our field
	
	// define constructors
	
	// define getter setters
	
	// define tostring
	
	// annotate fields
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="title")
	private String title;
	
	@ManyToOne(cascade= {CascadeType.PERSIST, CascadeType.MERGE, 
						CascadeType.DETACH, CascadeType.REFRESH})
	@JoinColumn(name="instructor_id")
	private Instructor instructor;
	
	@OneToMany(fetch=FetchType.LAZY, cascade=CascadeType.ALL)
	@JoinColumn(name="course_id")
	private List<Review> reviews;
	
	public Course() {
		
	}

	public Course(String title) {
		this.title = title;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public Instructor getInstructor() {
		return instructor;
	}

	public void setInstructor(Instructor instructor) {
		this.instructor = instructor;
	}

	public List<Review> getReview() {
		return reviews;
	}

	public void setReview(List<Review> reviews) {
		this.reviews = reviews;
	}
	
	// add a convenience method
	
	public void addReview(Review theReview) {
		
		if (reviews == null) reviews = new ArrayList<>();
		
		reviews.add(theReview);
	}

	@Override
	public String toString() {
		return "Course [id=" + id + ", title=" + title + "]";
	}
}
```
