# utl_most_popular_common_words_in_three_shakespeare_plays
Most popular common words in three Shakespeare plays.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Most popular common words in three Shakespeare plays

      WPS Proc Python solution

    Perhaps the word 'be' and the ngram 'to be' are very common in Shakespeare Plays.

    github
    https://github.com/rogerjdeangelis/utl_most_popular_common_words_in_three_shakespeare_plays


    StackOverflow Python:
    https://stackoverflow.com/questions/49277926/python-tf-idf-algorithm

    J Doe profile
    https://stackoverflow.com/users/8187340/j-doe

    This is an over simplified example.
    Python seems to the best language for A!?


    INPUT (Three Shanspeare plays in three SAS datasets)
    =====================================================

     Three Shakespeare Plays

        Play1, Play2 and Play3

     SD1.PLAY1  total obs=1    TXT

        Tis one thing to be tempted another thing to fall

     SD1.PLAY2  total obs=1    TXT

        To be or not to be that is the question

     SD1.PLAY3  total obs=1    TXT

        I must be cruel only to be kind

     Algorithm

        Discover words that are common to his plays but discount words like The, a ....
        Assign a weight to the words.

     Example output

     WORK.WANT total obs=18

     Obs    VARIABLE    LABEL         SUM

       1    BE          BE          1.07145
       2    TO          TO          1.00521   * even though TO is used more often(5 times)
                                                then 'BE'(4 times) it is discounted because
       3    THING       THING       0.61016     it has less meaning
       4    CRUEL       CRUEL       0.41724

    PROCESS (Python working code)
    =============================

    text = [txt1.iloc[0,0],txt2.iloc[0,0],txt3.iloc[0,0]];
    def tokenize(text):;
    .    tokens = word_tokenize(text);
    .    stems = [];
    .    for item in tokens: stems.append(PorterStemmer().stem(item));
    .    return stems;
    vectorizer = TfidfVectorizer();
    matrix = vectorizer.fit_transform(text).todense();
    matrix = pd.DataFrame(matrix, columns=vectorizer.get_feature_names());

    ods output summary=havsum;
    proc means data=wantwps stackods sum;
    var _numeric_;
    run;quit;

    proc sort data=havsum out=want;
    by descending sum;
    run;quit;

    *            _               _
      ___  _   _| |_ _ __  _   _| |_ ___
     / _ \| | | | __| '_ \| | | | __/ __|
    | (_) | |_| | |_| |_) | |_| | |_\__ \
     \___/ \__,_|\__| .__/ \__,_|\__|___/
                    |_|
    ;

    Python Output

    WANT.WANTWPS total obs=3

    Obs      BE       TO      CRUEL     ....     THE     THING     TIS

     1    0.18019  0.36037   0.00000    ....   0.00000  0.61016  0.30508
     2    0.39841  0.39841   0.00000    ....   0.33728  0.00000  0.00000
     3    0.49286  0.24643   0.41724    ....   0.00000  0.00000  0.00000


    WORK.WANT total obs=18

       VARIABLE    LABEL         SUM

       BE          BE          1.07145
       TO          TO          1.00521
       THING       THING       0.61016

       CRUEL       CRUEL       0.41724
       KIND        KIND        0.41724
       MUST        MUST        0.41724
       ONLY        ONLY        0.41724
       IS          IS          0.33728
       NOT         NOT         0.33728
       OR          OR          0.33728
       QUESTION    QUESTION    0.33728
       THAT        THAT        0.33728
       THE         THE         0.33728
       ANOTHER     ANOTHER     0.30508
       FALL        FALL        0.30508
       ONE         ONE         0.30508
       TEMPTED     TEMPTED     0.30508
       TIS         TIS         0.30508

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.play1 sd1.play2 sd1.play3;

      txt="Tis one thing to be tempted another thing to fall";  output sd1.play1;
      txt="To be or not to be that is the question  ";          output sd1.play2;
      txt="I must be cruel only to be kind";                    output sd1.play3;

    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64("
    libname wrk sas7bdat '%sysfunc(pathname(work))';
    options set=PYTHONHOME 'C:\Users\backup\AppData\Local\Programs\Python\Python35\';
    options set=PYTHONPATH 'C:\Users\backup\AppData\Local\Programs\Python\Python35\lib\';
    libname sd1 'd:/sd1';
    proc python;
    submit;
    import pandas as pd;
    import scipy;
    from sklearn.feature_extraction.text import TfidfVectorizer;
    from nltk import word_tokenize;
    from nltk.stem.porter import PorterStemmer;
    txt1 = pandas.read_sas('d:/sd1/play1.sas7bdat',encoding='ascii');
    txt2 = pandas.read_sas('d:/sd1/play2.sas7bdat',encoding='ascii');
    txt3 = pandas.read_sas('d:/sd1/play3.sas7bdat',encoding='ascii');
    text = [txt1.iloc[0,0],txt2.iloc[0,0],txt3.iloc[0,0]];
    def tokenize(text):;
    .    tokens = word_tokenize(text);
    .    stems = [];
    .    for item in tokens: stems.append(PorterStemmer().stem(item));
    .    return stems;
    vectorizer = TfidfVectorizer();
    matrix = vectorizer.fit_transform(text).todense();
    matrix = pd.DataFrame(matrix, columns=vectorizer.get_feature_names());
    top_words = matrix.sum(axis=0).sort_values(ascending=False);
    endsubmit;
    import  python=matrix data=wrk.wantwps;
    run;quit;
    ");

    ods output summary=havsum;
    proc means data=wantwps stackods sum;
    var _numeric_;
    run;quit;

    proc sort data=havsum out=want;
    by descending sum;
    run;quit;

