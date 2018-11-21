# [Python API—모범 사례](https://www.youtube.com/watch?v=oBYNyOyMGC4)

> 이제 Shotgun API를 사용할 준비가 되었으니 Shotgun 팀이 수집한 모범 사례를 살펴보고 API 실력을 높여 보십시오.

Developers use the shotgun Python API to create, find, update and delete entities in shotgun.
개발자는 Shotgun Python API를 사용하여 엔티티를 생선, 검색, 업데이트 및 삭제 합니다.

To upload and download files and to make changes to a shotgun sites database schema.
파일을 업로드 및 다운로드하고 숏건 사이트 데이터베이스 스키마를 변경합니다.

Using the API to automate repetitive tasks can save enormous amounts of human labor but there are also ways to abuse the API to create scripts that take minutes hours or even days to run.
API를 사용하여 반복 작업을 자동화할 경우 엄청난 양의 인적 작업이 절약될 수 있지만 API를 잘못 사용하면 몇 시간 또는 며칠이 걸리는 스크립트를 작성할 수도 있습니다.

Clogging up bandwidth at your studio creating an unwieldly Postgres database and decreasing performance on your shotgun site.
스튜디오에서 대역폭을 클로징하면 Postgres 데이터베이스가 생성되고 샷건 사이트에서 성능이 저하됩니다.

In this video we're going to review best practices for the shotgun Python API.
이 비디오에서는 숏건 Python API의 모범 사례를 살펴보겠습니다.

The common mistakes developers make and additional strategies that will help you get the most out of the API.
개발자가 흔히 저지르는 실수 및 추가 전략을 통해 API를 최대한 활용할 수 있습니다.

## 보일러 플레이트 CLI

I'm going to create a simple Python script with a CLI and promote some general Python best practices.
CLI를 사용하여 간단한 Python 스크립트를 만들고 일반적인 Python 모범 사례를 홍보하려고 합니다.

CLI stands for command-line interface.
CLI는 명령줄 인터페이스를 나타냅니다.

First grade a file with a Python `py` extension I'll call this one `best_practices.py`.
`best_practices.py` 만듭니다.

```python
# coding:utf-8

import os
import sys
import logging
import argparse
from datetime import datetime

_DESCRIPTION = "This script runs various tests to demonstrate efficent use of the Shotgun Python API."


def _run(args):
    """
    A boilerplate function to add code to.
    :param args: Arguments that control script behavior.
    """

    logging.info("Running.")


def _set_up_parser():
    """
    :return: dict of args.
    """

    # Initialize a command-line argument parser.
    parser = argparse.ArgumentParser(
        description=_DESCRIPTION
    )

    # Add an argument to the parser.
    parser.add_argument(
        "-r",
        "--run",
        help="Run this script.",
        action="store_true",
        required=False,
    )

    # Script out script usage if no arguments are passed.
    if len(sys.argv) < 2:
        logging.info("Usage: {} --help".format(__file__))
        sys.exit()

    # Resolve parser arguments.
    return vars(parser.parse_args())


def _set_up_logging():
    """
    Creates logs directory and sets up logging-related stuffs.
    """

    # Create a logs directory if it doesn't exist.
    if not os.path.exists("logs"):
        os.makedirs("logs")

    # Create a date stamp var for stamping the logs.
    date_stamp = datetime.now().strftime("%Y_%m_%d_%H-%M-%S")

    log = os.path.join(
        os.path.dirname(__file__),
        "logs",
        "{}_{}.log".format(
            os.path.basename(__file__).rpartition(".")[0],
            date_stamp
        )
    )
    print log
    # Set the logging level.
    logging_level = logging.DEBUG
    logging.basicConfig(
        filename=log,
        level=logging_level,
        format="%(levelname)s: %(asctime)s: %(message)s"
    )

    logging.getLogger().addHandler(logging.StreamHandler())


if __name__ == "__main__":

    # Initialize our logger and create related folders/files.
    _set_up_logging()

    # Parse our user input and toss it in a dict.
    args = _set_up_parser()

    start = datetime.now()
    _run(args)
    logging.info(
        "_run function took {} to complete.".format(datetime.now() - start)
    )

```

`best_practies.py -r` 실행 하면 로고 파일이 만들어 집니다.

## 기본적인 실수

샷건에 접속 한번만 한다.

## 배치 커멘드

We can dramatically speed things up by avoiding redundant API calls like authenticating and connecting to shotgun over and over or querying the same project over and over.
동일한 프로젝트를 반복해서 쿼리하거나 인증하고 샷건을 연결하는 등의 중복 API 호출을 방지하여 작업 속도를 크게 높일 수 있습니다.

We can also get a huge boost in speed if we gather up our request commands in a list and then send them to shotgun at one time with a batch command.
또한 우리가 우리의 요청 명령을 목록으로 모아 배치 명령을 한번에 샷건에 보내면 속도가 크게 향상될 수 있다.

```python
sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one("Project", [["name", "is", "project X"]])

batch_data = []
for shot in range(1, 100):
    shot_name = "sh{}".format(str(shot).zfill(3))
    batch_data.append({
        "request_type": "create",
        "entity_type": "Shot",
        "data": {
            "code": shot_name,
            "project": project
        }
    })
sg.batch(batch_data)
```

## find_one 명령

![Local Image](/img/2_api/5_The_Python_API_Best_Practices/1.png)

## 이벤트 로그 감소

잘 짜여진 스크립트라 해도 베치 명령어를 사용하면 수행되는 작업 만큼 이벤트 로고가 생성 됩니다.
이는 시스템을 느리게 할수 있습니다.
커스텀 로그 이벤트 엔티티를 만들어서 베치 명령이 하나의 이벤트로 처리 하게 하면 이를 방지 할수 있습니다.

## 이벤트 로그 엔티티

Let's keep going on the topic of event log entries.

Early on in my time at shotgun I was asked to create a burnin graph attract tasks completed over time

I believed my only option was to query the event log table.

Here's what the code looks like.

Let's add that function to the parser and run it.

Not so bad right. Okay.

But what if I remove some of these filters.

Yeah getting a little sluggish.

That's because the event log table is typically the biggest data hog on a shotgun site.

You've already seen how many event log entries were created when we run batch commands.

All those event log entries have to be gathered up by the application server and then transferred over the network to your script.

And the longer you use your shotgun site the more event log entries could be returned they're rarely deleted.

Also this is a test site that no one uses except me 

and I hardly ever use it and it's still slow

A legit production site with tens or hundreds of users and dozens of projects produces heaps and heaps of event log entries.

The moral of the story is only query event log entries as a last resort.

And if you do have to query them try to be smart with your filters to prevent a ton of data from clogging the wire.

So event log entries with the API equals badness.

But what if I still need to build a report or graph from historical data or sleuth out the state of a field on a certain date.

Are there alternative ways to solve those problems.

Yes first take a look at the shotgun event daemon.

You'll find it in the shotgun software github repo titled Chuck and events.

The daemon watches the shotgun event log and can take action when specific events occur that way scripts run proactively instead of retroactive  Lee and the event log entry queue strategy is optimized.

I also encourage you to check out a repo called SG cron datapoint.

The script in this repo can be configured to write data point to a custom entity every night via crontab or other process scheduler.

At that point it's a simple matter to hit that custom data point entity to generate historical data crafts or reports.

Finally shotgun is always working on better solutions so it's likely that in the near future historical data tracking features will materialize.

As well as web hooks which could both solve issues you may otherwise attempt to solve with event log queries.

## schema_field_create

If you're writing a script that requires specific fields to exist on entity types.

Then you can of course use the API to check for those fields and add them as needed super convenient.

Alright a little bit of code here that updates a text field called SG monkey on any entity it comes across.

Since we know the monkey field won't exist initially this code will fail.

Let's verify that.

There you go.

We get a shotgun a pi/3 fault exception.

Okay cool.

Well let's add an exception handler to cover this case.

And add some code up here to create field if it doesn't exist.

What's wrong with this.

It seems a bit overzealous to run the schema field read function every time the script is run right.

So let's break the schema stuff out into its own function.

I'll call it setup schema and ask the user to run it if we hit an error.

Cool

Much better less magical more engineering.

And let's see it in action.

I'll try to update a monkey field on a sequence entity there's our nice little message.

Okay. Master script I'll run you with the setup flag just like you're asking sweet and now let's see if our update command works.

Great.

So the moral of the story here is while it is a good idea to automate schema set up those API checks and updates only need to be done once or at least infrequently.

So it's a good idea to think about separating your schema checks and modifications out into separate setup functions.

## find를 이용한 schema_field_read.keys()

While we're talking about schema calls.

I'm going to call out one of my pet peeve bad behaviors.

And I'm going to do that at the risk of introducing you to a bad habit.

As we already know when you query an entity in shotgun a minimal amount of information is returned the entities type and ID.

If you want more information for example what the entity's name is you've got to look up the internal field code and specify that explicitly in  he field commands third argument.

Now you clever people may have already found a shortcut which is this.

You can simply pass in the keys from a schema field read call and voila.

You get all the entities fields and field values in the payload.

Great right.

No super super bad.

Let's look at the timestamp here.

And now let's use the original method of specifying just the information we need.

Cool so that took this long and that's a big difference let's put up the numbers.

Wow but it makes sense right.

You're pulling a ton more data over the wire with the key strategy and our fake o shot field values are none for the most part which means the  call would be even slower with real production shots.

I hope you're glad to have the schema field read Keys trick up your sleeve maybe for messing around in the terminal but it will benefit you in the long run to learn the good habit of explicitly declaring your fields and not taking this shortcut.

## 유용한 단축키

While we're on the topic of shortcuts let's take a quick look at some of the less often used methods.

My favorite unused method is summarized while at a quick shortcuts function here and then count the number of shots in my project you.

Superfast.

Next let's get all replies to a specific note in chronological order which could be a pain with the standard crud calls.

Cool easy.

Now let's add thumbnails for a hundred shots by sharing a thumbnail from a single shot.

You nice so fast.

That would be super useful used in conjunction with some filtering.

For example I can share a truck thumbnail with any asset that had truck in the name.

I'll stop there but I urge you to look at the followers and activity stream read methods and also the new filters presets that were introduced in shotgun 7.0.

Like the latest preset that's a super useful one.

## 설명서

<https://support.shotgunsoftware.com/hc/ko/articles/219030848-%ED%95%84%ED%84%B0-%EB%B0%8F-API-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%B5%9C%EC%A0%81%ED%99%94%ED%95%98%EC%97%AC-Shotgun-%EC%82%AC%EC%9D%B4%ED%8A%B8-%EC%86%8D%EB%8F%84-%EA%B0%9C%EC%84%A0>

Before we conclude I'd like to highlight a relevant support doc.

It covers some of the same topics in this video but in a little more detail.

There's some great discussion about filtering.

I've also posted the script we created in this video here.

<https://github.com/shotgunsoftware/gists/blob/master/python_api/best_practices.py>

And finally I'd like to mention that although sharing a shotgun connection object is a good strategy.

It's not currently thread-safe as of February 2018.

So don't share a connection object across threads.

Check out these lines in github to see if that's changed.

<https://github.com/shotgunsoftware/tk-core/blob/master/python/tank/util/shotgun/connection.py#L340-L361>