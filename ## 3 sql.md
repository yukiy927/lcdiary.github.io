## 3 sql

```sql
/*
Enter your query below.
Please append a semicolon ";" at the end of the query
*/


-- 
-- sort by asc title, des jss, asc email

Select 
    p.title, p.primary_skill, f.email,
    f.skills_list, f.job_success_score
From 
    projects p left join (select * from freelancers where 
    job_success_score >= 0.85) as f
    on 
        f.skills_list like concat('%,', p.primary_skill)  # in the end
        or 
        f.skills_list like concat('%,', p.primary_skill, ',%') # in the middle
        or 
        f.skills_list like concat(p.primary_skill, ',%') # beginning
        or 
        f.skills_list = p.primary_skill # only 1 skill
Order BY
    p.title asc,
    f.job_success_score desc,
    f.email asc

```

## 4 optimal storage

```python
def getStoredWord(word, max_operations):
    # Write your code here
    if max_operations > 25:
        return "a" * len(word)
        
    already = set()
    set_word = []
    for i in word:
        if i not in already:
            set_word.append(i)
            already.add(i)
    
    idx = 0
    k = 0
    # able to complete in m steps
    idx_most = 0
    while idx < len(word) and (k_ := ord(set_word[idx]) - ord('a')) <= max_operations:
        if k_ > k:
            k = k_
            idx_most = idx
        idx += 1
    
    
    for i in range(idx):
        word = word.replace(set_word[i], 'a')
    origin = ord(set_word[idx_most])
    now = max(ord('a'), ord(set_word[idx_most]) - max_operations)
    
    for _ in range(now + 1, origin):
        word = word.replace(chr(_), chr(now))
        
    if idx < len(set_word):
        origin = ord(set_word[idx])
        now = ord(set_word[idx]) - max_operations + k
        word = word.replace(set_word[idx], chr(ord(set_word[idx]) - max_operations + k))
        
        for _ in range(now + 1, origin):
            word = word.replace(chr(_), chr(now))
    return word

```
