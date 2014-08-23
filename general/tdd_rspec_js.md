## rspec
### Mockup constructor
`allow(Pusher::Client).to receive(:new).with(app_id: ENV['PUSHER_APP_ID'], key: ENV['PUSHER_KEY'], secret: ENV['PUSHER_SECRET']).and_return(pusher_client)`
Above will affect all Pusher::Client's constructor. It may not be ordered

## js
`spyOn(pusher, 'subscribe').and.callThrough();
 spyOn(pusher, 'subscribe').and.callFake(function (channel) {
                    if (channel === 'private-project-10') {
                        return fakeChannel;
                    }
                });
 spyOn(currentProject, 'refreshFolders');
 fakeChannel.trigger();
 expect(currentProject.refreshFolders).toHaveBeenCalledWith('hello');`
Above are executed in order

## Multiple Type TDD
### BDD or feature test (e2e test, no mock)
### UI TDD (Javascript with mock, mainly verify the UI behaves as designed, e.g., a modal dialog is popup, with right message when error happen)
### NonUI TDD (Rspec with  mock, verify the right function is called at right time)

## Design philosophy
* Controller prepare the dependency
* service do business logic
* service can be easily mocked for testing (avoid static property)

## Example
We want test folder rename can trigger a push from server to client. To do that, we need following tests:
* feature (e2e) behavoir: the folder tree will show only the new folder and the old folder gone
* frontend(js): subscribe to the channel, and call the right mothod to refresh the UI
* backend(ruby): receive rename folder event fro p4d and then trigger pusher to broadcast the event

Remember, when we test, we whitebox testing.
