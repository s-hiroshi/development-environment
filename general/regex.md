PhpStorm
=====

## コード整形

### コメント1

    <!-- ?/([a-z0-9-_]+) ?-->

    <!-- $1 -->


before

    <!-- /id_or-scelector -->

after

    <!-- id_or_selector -->


### コメント2

    ^( *)(<!-- /?[a-zA-Z0-9-_]+ -->)(.*)$


    $1$3$2

before

    <!-- content --></div>

after

    </div><!-- content -->

### ()

    \(([a-zA-z0-9$]+)\)

    ( $1 )

before

    if ($example) {

    if ( $example ) {

### フォーマット

    \(([^ )]+)

    ( $1