## IO vs NIO vs NIO2

New IO
Stream을 사용하는 것이 아니라 channel과 Buffer를 사용한다.
IO 대상(File, 객체 등)에 대한 channel을 열고
Buffer에 내용을 쓰고 이 buffer를 channel로 넘기면 알아서 해준다
반대로 읽을 때는 IO 대상에 대한 channel을 열고 읽어온다.

