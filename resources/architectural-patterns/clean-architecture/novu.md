https://github.com/novuhq/novu/blob/c7f68d37898bd59dbdd4f3067b3398805e7c799c/apps/api/src/app/user/usecases/get-my-profile/get-my-profile.usecase.ts#L6

Novu is an open-source notification infrastructure. The project is a huge mono repo. Understandably, they need a way to help tie everything together. Clean architecture seems to be one of them.

For example, to get a user profile, it's [`controller`](https://github.com/novuhq/novu/blob/next/apps/api/src/app/user/user.controller.ts#L49)

```ts
  @Get('/me')
  @ApiResponse(UserResponseDto)
  @ApiOperation({
    summary: 'Get User',
  })
  @ExternalApiAccessible()
  async getMyProfile(@UserSession() user: IJwtPayload): Promise<UserResponseDto> {
    Logger.verbose('Getting User');
    Logger.debug('User id: ' + user._id);
    Logger.verbose('Creating GetMyProfileCommand');

    const command = GetMyProfileCommand.create({
      userId: user._id,
    });

    return await this.getMyProfileUsecase.execute(command);
  }
```

Then [`usecase`](https://github.com/novuhq/novu/blob/next/apps/api/src/app/user/usecases/get-my-profile/get-my-profile.usecase.ts#L6)

```ts
@Injectable()
export class GetMyProfileUsecase {
  constructor(private readonly userRepository: UserRepository) {}

  async execute(command: GetMyProfileCommand) {
    //...

    await this.userRepository.update(
        { _id: profile._id },
        {
          $set: {
            'servicesHashes.intercom': userHashForIntercom,
          },
        }
      );
    
    // ...
  }
}
```

Then [`repository`](https://github.com/novuhq/novu/blob/next/libs/dal/src/repositories/base-repository.ts#L156C9-L156C16)

```ts
  async update(
    query: FilterQuery<T_DBModel> & T_Enforcement,
    updateBody: UpdateQuery<T_DBModel>
  ): Promise<{
    matched: number;
    modified: number;
  }> {
    const saved = await this.MongooseModel.updateMany(query, updateBody, {
      multi: true,
    });

    return {
      matched: saved.matchedCount,
      modified: saved.modifiedCount,
    };
  }
```