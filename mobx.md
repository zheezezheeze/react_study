# Mobx의 비동기 작업
>Mobx의 @action은 현재 사용중인 함수에만 영향을 주고
이 함수에 스케줄된 다른 함수는 영향을 받지 않기 때문에
다른 상태변화가 일어나는 콜백함수들은 또다른 action으로 감싸져야 한다.

##1. Promise
```javascript
mobx.configure({ enforceActions: 'observed' })

class Store {
  @observable githubProjects = []
  @observable state = 'pending'
  
  @action
  fetchProject() {
    this.githubProjects = []
    this.state = 'pending'
  
    fetchGithubProjectSomehow().then(
      action('fetchSuccess', projects => {
        const filteredProjects = somePreprocessing(projects)
        this.githubProjects = filteredProjects
        this.state = 'done'
      }),
      action('fetchError', error => {
        this.state = 'error'
      })
    )
  }
}
```

##2. runInAction : 상태변화를 일으키는 콜백action만 따로 적용하는 방법 
```javascript
mobx.configure({ enforceActions: 'observed' })

class Store {
  @observable githubProjects = []
  @observable state = 'pending'
  
  @action
  fetchProject() {
    petchProjects() {
      this.githubProjects = []
      this.state = 'pending'
      fecthGithubProjectsSomehow().then(
        projects => {
          const filteredProjects = somePreprocessing(projects)
          runInAction(() => {
            this.githubProjects = filteredProjects
            this.state = 'done'
          })
        },
        error => {
          runInAction(() => {
            this.state = 'error'
          })
        }
      )
    }
  }
}
```

##3. async / await
```javascript
mobx.configure({ enforceActions: 'observed' })

class Store {
  @observable githubProjects = []
  @observable state = 'pending'
  
  @action
  async fetchProjects() {
    this.githubProjects = [];
    this.state = 'pending'
    try {
      const projects = await fetchGithubProjectsSomehow()
      const filteredProject = somePreprocessing(projects)
      runInAction(() => {
        this.state = 'done'
        this.githebProjects = filteredProjects
      })
    } catch (error) => {
      runInAction(() => {
        this.state = 'error'
      })
    }
  }
}
```

##4. flows : decorator 사용X
```javascript
mobx.configure({ enforceActions: 'observed' })
class Store {
  @observable githubProjects = []
  @observable state = 'pending'
  
  fetchProjects = flow(function * () {
    this.githubProjects = []
    this.state = 'pending'
    try {
      const projects = yield fetchGithubProjectsSomehow()
      const filteredProjects = somePreprocessing(projects)
      
      this.state = 'done'
      this.githubProjects = filteredProjects
    } catch (error) {
      this.state = 'error'
    }
  })
}
```
