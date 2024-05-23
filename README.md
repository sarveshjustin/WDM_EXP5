```


    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros((num_docs, num_terms), dtype=int)

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()
            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(self.documents_matrix, columns=self.index.keys())
        print(df)

    def print_all_terms(self):
        print("All terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        query_terms = query.lower().split()
        results = None

        for term in query_terms:
            doc_ids = self.index.get(term, set())
            if results is None:
                results = doc_ids.copy()
            else:
                if term.startswith('not'):
                    results.difference_update(doc_ids)
                elif term == 'or':
                    results.update(doc_ids)
                elif term == 'and':
                    results.intersection_update(doc_ids)

        return list(results) if results else []
```
