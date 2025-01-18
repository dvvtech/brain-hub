api контроллер возвращает Dto объекты и принимает реквест в виде Dto
репозитории могут возвращать Entity и в слое сервисов Entity мапятся на модели

- [ ] Core  (Domain)                           или папка или проект 
    - [ ] Models                                доменная моlель
            Cards
             Person


- [ ] Models    (Contracts)
                                   (это объекты возвращаемые контроллерами можно и без Dto на конце или приписать Model)
         CouponDto.cs
         ResponseDto.cs
- [ ] Data
      AppDbContext.cs      
     - [ ]  Entities
          CouponEntity.cs                          
- [ ] Migrations
- [ ] Repositories              пусть репозиторий возвращает Entity
- [ ] Mapper
        ModelConverter.cs    static class + static method DtoModel
- [ ] Services
      UserSevices.cs  - методы будут возвращать Dto объекты + IEnumerable
- [ ] Configuration
    EmailSettings.cs or EmailConfig.cs
      Constants.cs
///////////////////////////////////////////////////////////////////////////////

- [ ] ApiGateway
    - [ ] Ocelot.ApiGateway
- [ ] Infrastructure
    - [ ] EventBus.Messages
         - [ ] Common
         - [ ] Events         
- [ ] Services
    - [ ] Basket    
        - [ ] Basket.API
        - [ ] Basket.Application
        - [ ] Basket.Core
        - [ ] Basket.Infrastructure

//////////////////////////////////////////////////////////////////////////////////////////////
- [ ] src
    - [ ] Core
        - [ ] Models
        - [ ] Services
        - [ ] Interfaces
            - [ ] IEmailSender
            - [ ] IDataContext
    
    - [ ] Infrastructure           тут могут быть Repository DbContext Cached Repositories   API Clients
                      File system, asure storage, sms implementation
        - [ ] Data             реализация слоя доступа к данным
        - [ ] Email           реализация разных способов отправки писем

    - [ ] UseCases             тут может быть реализован CQRS  Commands Queries DTOs Handlers(for command and queries)
        - [ ] Users
            - [ ] Create
            - [ ] Detete
- [ ] tests
- [ ] docker-compose

/////////////////////////////////////////////////////////////////////////////////////////////////////////////
https://www.youtube.com/watch?v=YiVqwoFMieg
- [ ] src
    - [ ] Applications
        - [ ] Common
            - [ ] Models
            - [ ] Mapping
                IApplicationDbContext.cs
        - [ ] TodoItems
            - [ ] Commands
                - [ ] CreateTodoITem
            - [ ] Queries
            - [ ] EventHandlers
        - [ ] TodoList
            - [ ] Command
            - [ ] Queries
    - [ ] Domain
        - [ ] Common
              Base classes for entity
        - [ ] Entities
              TodoItem.cs
              TodoList.cs
        - [ ] Enums
        - [ ] Events
               TodoItemCreatedEvent.cs
               TodoItemCompletedEvent.cs
        - [ ] Exceptions
    - [ ] Infrastructure
        - [ ] Files
        - [ ] Identity
        - [ ] Persistence
            - [ ] Migrations
            - [ ] Configurations
                 ApplicationDbContext.cs
        - [ ] Services
    - [ ] WebUI
- [ ] tests
    - [ ] IntegrationTests
    - [ ] Application.UnitTests
    - [ ] Domain.UnitTests
