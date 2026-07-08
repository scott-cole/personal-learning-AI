# DAY10: Checkpoint

**Closed book.**

1. What's the difference between the service layer and the repository layer?
   the repository layer handles storage the service layer handles business logic and doesnt care where it gets the data from

2. What is dependency injection? Why does the service receive the repository instead of creating its own?
   this recieves the repo so that it doesnt care where the storage is stored, it can be anywhere and the code doesnt have to change. dependency injection is passing in the repo so no matter what is passed the service code wouldnt change

3. In the `Shorten` function, why do we validate the URL format before generating a code?
   this early return, if the url isnt valid theres no point generating a code

4. The service returns `storage.URL`. Is this coupling the service to the storage package? Why might this be a problem?

Yes, returning storage.URL couples the service to the storage package. The problem: if storage.URL changes (rename a field, add a tag), the service must change too. Also the service can't exist without importing storage.
The fix: define your domain model in a shared package (e.g. domain/ or models/) that both service and storage import, so storage depends on the model, not the other way around.

5. How would you write a unit test for the `Shorten` function without needing a real repository?

type mockRepo struct {
savedURL storage.URL
}

func (m \*mockRepo) Save(url storage.URL) error {
m.savedURL = url
return nil
}

func (m *mockRepo) FindByCode(code string) (storage.URL, error) {
return storage.URL{}, nil
}
Then inject it:
func TestShorten_ValidURL(t *testing.T) {
repo := &mockRepo{}
svc := service.NewService(repo)

    result, err := svc.Shorten("https://example.com")
    // assert no error, assert code is 8 chars, assert saved URL matches

}
