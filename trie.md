#
[Source](https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems "Permalink to Tutorial on Trie and example problems - Threads @ IIIT Hyderabad")

# Hướng dẫn về Trie (cây tiền tố) và các ví dụ

Tôi sẽ viết trong bài viết này về Tries và khái niệm tổng quát được sử dụng trong các vận dụng nhỏ của vấn đề. Chúng ta sẽ thấy 2-3 vấn đề mà trie hữu ích.

Đầu tiên chúng ta xem xem trie là gì. Trie có thể lưu thông tin về keys/numbers/strings nhỏ gọn trọng một cây.  
Trie bao gồm các node, nơi mỗi node lưu trữ một ký tự hoặc một bit. Chúng ta có thể thêm các chuỗi hoặc số phù hợp.  
Đây là một ví dụ trie về chuỗi:

![1](https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd)

  
Nguồn: Wikipedia.  

Nhưng chúng ta sẽ xử lý các con số ở đây, và đặc biệt trong các bit nhị phân. Chúng ta sẽ thấy việc chúng ta giải quyết các vấn đề.

**Vấn đề 1**: Cho một mảng các số nguyên, chúng ta phải tìm 2 phần tử mà XOR  cho gía trị lớn nhất.
**Giải quyết:**  
Gỉa sử chúng ta có một cấu trúc dữ liệu thỏa mãn hai loại truy vấn:   
1\. Chèn một số X    
2\. Cho một số Y, tìm max XOR của Y với tất cả số mà đã được thêm cho đến giờ.

Nếu chúng ta có cấu trức dữ liệu này, chúng ta sẽ thêm các số nguyên, và với truy vấn của loại thứ 2 chúng ta sẽ tìm max XOR.  
Trie là cấu trúc dữ liệu mà ta sẽ sử dụng. Trước tiên, hãy xem cách mà ta chèn các phần tử vào trie.


![2](https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c)

  
Vậy, chúng ta sẽ theo dấu vết đưòng đi của những số mà ta cần chèn.chúng ta không phải vẽ lại đường đi hiện tại.  

Chèn một key có độ dài N cần O(N) là log2(MAX) trong đó MAX là số tối đa số được chèn trong trie, bởi vì có tối đa log2(MAX) bit nhị phân trong số.  
Bằng cách này, chúng ta lưu trữ tất cả dữ liệu về tất cả các số được chèn vào trie đến bây giờ.  
Bây giờ, cho truy vấn của loại 2:
Đặt số Y của chúng ta là b1,b2...bn, trong đó b1,b2.. là các bit nhị phân. Chúng ta bắt đầu từ b1. Bây giờ cho XOR là maximum, chúng ta sẽ cố gặng tạo bit 1 quan trọng nhất sau khi dùng XOR. Vì vậy, nếu b1 là 0, chúng ta sẽ cần 1 và ngược lại. Trong trie, chúng ta đi đến vùng bit được yêu cầu. Nếu tuỳ chọn thuận lợi không được, chúng ta sẽ chuyển sang vùng khác. Làm điều này cho i=1 đến n, chúng ta sẽ lấy maximum XOR khả thi.  

![3](https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461)

Truy vấn quá log2(MAX).

**Vấn đề 2**: Cho một mảng số nguyên, tìm mảng con với maximum XOR.  
**Giải quết:**  
Giả sử F(L,R) là XOR mảng con từ L đến R.  
Ở đây chúng ta dùng thuộc tính mà F(L,R)=F(1,R) XOR F(1,L-1). Làm thế nào?
Giả sử mảng con với maximum XOR kết thúc tại vị trí i. Bây giờ, chúng ta cần tối đa F(L,i) ie. F(1,i) XOR F(1,L-1) where L<=i. Giả sử, chúng ta đã thêm F(1,L-1) vào trie của chúng ta cho mọi L<=i, thì đó là vấn đề 1


    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thể thử vấn đề này ở đây: [ACM-ICPC Live Archive](https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683)  

**Vấn đề 3**: Cho một mảng các số nguyên dương mà bạn phải in số  mảng con mà XOR của chúng nhỏ hơn K.

**Giải quyết:**  
Giải pháp này sử dụng lại các khái niệm mà ta đã thấy cho tới giờ. Chúng ta cứ xử lí như câu hỏi trưóc đó:
Với từng index i=1 tới N, ta có thể đếm xem có bao nhiêu mảng con kết thúc tại vị trí thứ i mà thỏa mãn điều kiện được đưa ra  

    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  
query(q,k) trả về số các số nguyên đã tồn tại trong cấu trúc mà khi XOR và q trả về một số nguyên nhỏ hơn k.  
Chúng ta so sánh các bit tương ứng của q và k, bắt đầu từ các bit quan trọng nhất. giả sử p và q là các bit tương ứng mà chúng ta đang xem xét.

Nếu q là 1, và p là 0, chúng ta thực hiện điều này:   

![4](https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25)  

Tương tự chúng ta có thể dễ dàng giải ra 3 trường hợp khác (q=1,p=1), (q=0,p=1) and (q=0,p=1).
Vậy, chúng ta cần thay đổi cấu trúc của chúng ta ở đây, chúng ta cũng gĩư một số node lá mà có thể tiếp cạn từ node hiện tại nếu ta đi từ phía bên trái và tương tự đối với bên phải. Bởi vì, mặt khác đô phức tapj sẽ tăng dần, nếu chúng ta duyệt cây lặp đi lặp lại. Chúng ta có thể làm điều này trong khi thêm số và trong cây một cách dễ dàng.  

Vấn đề này đưọc đặt trong CodeCraft'14. Bạn có thể thực hành tại đây: [SPOJ.com - Problem SUBXOR](http://www.spoj.com/problems/SUBXOR)  

Bây giờ, hãy nói về việc triển khai.  
Để triển khai code cho trie trong C/CPP ta có thể giữ các node và các con trỏ trái và phải. Ta có thể viết hàm đệ quy.

    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn cũng vậy, ta duyệt đệ quy trên cây.

Cập nhật:  
Một vấn đề khác sử dụng trie (yay! :P).  
[Problem - B - Codeforces](http://codeforces.com/contest/455/problem/B)  

**Vấn đề con**: Cho một nhóm n chuỗi không rỗng. Trong trò chơi, hai người chơi cùng xây dựng từ cùng nhau, khởi đầu thì từ rỗng. Người chơi chơi theo lượt. Trong lượt của mình, người chơi phải thêm một chữ cái vào cuối của từ, kết quả của từ phải là tiền tố của ít nhất một chuỗi trong nhóm. Một người chơi sẽ thua nếu anh ta không thể thực hiện bước đi của mình.  

Ta cần tìm người chơi nào (1 hoặc 2) mà có khả năng chiến thắng.  

Vậy, ý tưởng ở đây lại là xây dựng một trie chứa các chuỗi. Tại sao? Bởi một trie luôn lưu trữ thông tin về tất cả các tiền tố. Bây giờ, ta sẽ thử đánh giá xem với mỗi node liệu người chơi đầu tiên có khả năng giành thắng lợi hay không. Ta có thể thực hiện điều này theo cách đệ quy. Với node v, từng node u mà u là con trực tiếp của v, nếu người chơi đầu tiên có khả năng thua vì u, thì với node v người chơi đầu sẽ có khả năng giành chiến thắng.  
Ví dụ, giả sử ta có "abc", "abd", "acd".  
Trie của chúng ta sẽ nhìn như sau:  

![5](https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c)  

Tất cả node lá đều có khả năng chiến thắng.
