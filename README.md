import stanza
import csv
from collections import Counter

stanza.download('en')
nlp = stanza.Pipeline(lang='en', processors='tokenize,mwt,pos,lemma,depparse')

with open(r"yourfilepath.txt", "r", encoding="utf-8") as f:
    sentences = f.readlines()

dep_counter = Counter()
total_deps = 0

for sent in sentences:
    doc = nlp(sent.strip())
    for sentence in doc.sentences:
        for word in sentence.words:
            rel = word.deprel
            dep_counter[rel] += 1
            total_deps += 1

results = []
for rel, count in dep_counter.items():
    proportion = round(count / total_deps, 3)
    results.append((rel, count, proportion))

results.sort(key=lambda x: x[1], reverse=True)

output_path = r"yourfilepath.csv"
with open(output_path, "w", newline='', encoding="utf-8") as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow(["Dependency_Type", "Count", "Proportion"])
    writer.writerows(results)

print(f"✅ 分析完成，结果已保存至：{output_path}")

